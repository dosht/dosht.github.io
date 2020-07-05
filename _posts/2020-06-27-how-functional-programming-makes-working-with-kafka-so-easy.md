---
layout: post
title:  "How Functional Programming Makes Working with Kafka Easier?"
author: mou
categories: [ scala, kafka ]
image: assets/images/2020-06-27-how-functional-programming-makes-working-with-kafka-so-easy.png
featured: true
hidden: false
sitemap: true
---


When your system starts to grow and split into multiple services, you will presumably need a way to send messages between those services for processing. In a clean and nice architecture, each service should have a bounded context that defines its space of responsibility and other requirements outside of that context should be handled by other components including messages delivery, so the service will only emit a message (fire-and-forget) and other services that are interested in processing those messages will catch them.

A message queue (publish-subscribe-based) will be a good fit here. And in my opinion, Kafka should not be the default go-to choice when you think of a pub-sub because of its complexity and other solutions are designed to simplify the development and infrastructure work with reasonable trade-offs such as GCP PubSub, AWS Kinesis, or NATS, however, if none of them met your requirements and you want to deploy Kafka, then there are a few things you need to take care of.

In this blog post, We will build a simple and powerful Scala client in a functional style that solves some of hidden Kafka client difficulties and potentially simplifies testing, error handling, and other aspects as you will see later in the companion project.

# Kafka with Scala

## What is Kafka
Kafka is - a publish-subscribe based durable messaging system exchanging data between processes, applications, and servers. [<sup>1</sup>][Kafka Definition]

Kafka consists of two sides: A producer that produces messages to a topic and a consumer that subscribes to a topic and consumes messages from that topic. Multiple consumers can subscribe to the same topic and poll all messages from it as long as they belong to different consumer groups. Each consumer group has an offset that points at the position in the topic where it consumed until.

## Kafka Clients that Works with Scala
This is not meant to be a full comparison, so it's enough to list some available clients with a couple of words about each of them.

1. Apache Kafka (Java) Client: This is the [official Java client][Apache Kafka Client]. It has a Java API and I couldn't find a Scala wrapper for it to make functional or thread-safe, ..etc
2. [CakeSolutions client][CakeSolutions client]: It's a well-written Akka-based client. I used also this client myself since I was working in [Cake Solutions][cakesolutions]. My former colleague David Piggott has a nice [blog post][David's Post] about it, but that was before it has been acquired by Disney and I'm not sure how much they're still maintaining it.
3. [Alpakka project][Alpakka project]: This client is based on Akka stream. I used this one also in a previous project and fits well if you have already Akka streams application. It's helpful to design complex streaming graphs involving many Kafka topics and it helps also in managing offsets and applying back-pressure.
4. [Lagom Kafka Client][Lagom Kafka Client]: This based on Alpakka client, but it's designed to work with Lagom

## Apache Kafka Client

Let's first start with the vanilla Java-based official client and see how we will enhance it with [Cats][Cats] and [Cats Effect][Cats Effect] to make it more compatible with Scala and functional programming.

The simple imperative code looks like this <sup>[2][Kafa client tutorial]</sup>:

```scala
def consumeFromKafka(topic: String) = {
  val props = new Properties()
  props.put("bootstrap.servers", "localhost:9094")
  props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer")
  props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer")
  props.put("auto.offset.reset", "latest")
  props.put("group.id", "consumer-group")
  val consumer: KafkaConsumer[String, String] = new KafkaConsumer[String, String](props)
  consumer.subscribe(util.Arrays.asList(topic))
  while (true) {
    val record = consumer.poll(1000).asScala
    for (data <- record.iterator)
      println(data.value())
  }
}
```

![1][1] THIS IS TOO JAVA! 

# Cats with Kafka

[Cats][Cats] is a library that provides type classes and data structures for functional programming in Scala like Monad, Semigroup, Monoid, Applicative, ..etc and [Cats Effect][Cats Effect] that mainly provides the IO monad that describes an operation that performs a side effect where the result of the effect can be returned synchronously or asynchronously. With [Cats][Cats] and [Cats Effect][Cats Effect], you can define all the logic of your program without running anything (including operations that have side effects) and separately run the program (or part of it) synchronously or asynchronously. This will help us to improve testability and maintainability and help understand potential sources of errors.

## Kafka Context

### Apache Kafka Client is not Thread-Safe

As you saw above, the consumer is wrapped in `while(true)` block which runs in a single thread. You need to handle messages in multiple threads for better utilization, but if you try to call `kafkaConsumer.poll` on a thread other than the one that it was defined at, you will get an exception. To solve this, we will keep calling `kafkaConsumer.poll` in the same thread and we will use another thread pool for handling messages. Let's see how this is easy with cats.

### Cats ContextShift

Cats provides `ContextShift` as the [pure][pure function] alternative of Scala's `ExecutionContext`. It always you to switch thread-pools while performing flatmap operations, so you can do something like this:

```scala
val ec: ExecutionContextExecutor = ExecutionContext.global
val cs: ContextShift[IO] = IO.contextShift(ec)

for {
    _ <- cs.shift
    a <- f1
    b <- cs.evalOn(anotherExecutionContext)(f2)
    c <- f3
} yield (a, b, c)
```

`f1` will run on the `global` execution context and before running `f2`, `cs` will switch to `anotherExecutionContext` and run f2 on it and then switch back to `global` execution context and run `f3`.

### Kafka Single Thread with Cats Effects

Now, we will define a thread pool with a fixed number of 1 thread to initialize `kafkaConsumer` and call `.poll` and `.close` and we will use `ContextShift` to switch between this singular thread pool and the other thread pool used for handling messages which can be anything.

**`KafkaContext.scala`**

```scala
class KafkaContext(cs: ContextShift[IO]) {
  private val pool: ExecutorService = Executors.newFixedThreadPool(1, new ThreadFactoryBuilder().setNameFormat("kafka-thread").build())

  protected val synchronousExecutionContext: ExecutionContextExecutor = ExecutionContext.fromExecutor(pool)

  def execute[A](f: => A): IO[A] = cs.evalOn(synchronousExecutionContext)(IO(f))

  // Alias for execute
  def ~>[A](f: => A): IO[A] = execute(f)

  def close(): IO[Unit] = IO(pool.shutdown())
}
```
### Building the Consumer

Now, we can create the Kafka consumer in the Kafka thread that we just defined in `KafkaContext`. You can do this simply by creating a new instance of `KafkaConsumer` and wrap it in `KafkaContext.execute`:

```scala
kafkaContext.execute(new KafkaConsumer(...))
```

Since we need to close the Kafka consumer after we don't need it anymore, it's more suitable to define it as a closable resource and Cats provide a resource abstraction for that called [`Resource`][cats resource] used for acquiring and releasing resources using `Resource.make` function that takes 2 functions: `acquire` and `release` and returns a `Resource` instance where you can call `use` that will allow you to use the resource and make sure to release it after you're done with it. We can do the same thing `KafkaContext`.

```scala
def createKafkaConsumer = kafkaContext ~> new KafkaConsumer(...)

def closeKafkaConsumer(kafkaConsumer: KafkaConsumer[...]) = kafkaContext ~> kafkaConsumer.close()

val kafkaConsumerResource = Resource.make(createKafkaConsumer)(closeKafkaConsumer)
kafkaConsumerResource.use { kafkaConsumer =>
  kafkaContext ~> kafkaConsumer.subscribe(Collections.singletonList(config.topic))
}
```

Here we defined `kafkaConsumerResource` that creates and closes the consumer and use it in the same thread. Next, it will queue messages to another thread pool with probably more than one thread for processing.

### Handling messages in another thread pool

This will not be so difficult at all since `ContextShift.evalOn` already switches back to the original execution context. The client code that uses `kafkaConsumerResource` can specify the execution context where the messages are handled. The following code uses simple for comprehension to poll messages from the Kafka topic, process them and then poll next messages.

```scala
def consume: IO[Unit] = for {
    messages <- kafkaContext ~> kafkaConsumer.poll(Duration.ofMillis(1000)).iterator().asScala.toList
    _ <- messageHandler.handleMessage(messages)
    _ <- consume
  } yield ()
```

Here, only `kafkaConsumer.poll` will run in the Kafka thread and `messageHandler.handleMessage(messages)` will run in another thread pool decided on the caller of `consume` function. This is a functional alternative to `while(true)` used in the vanilla example.

Note that we called `consume` in a non-tail-recursive position, but it's [stack safe][thread safety] means it will not throw `StackOverFlowError`. Remember that `consume` function doesn't actually run anything, it just describes the task, so it lets `cats.IO` decide how to execute it and `cats.IO.flatMap` runs in a [trampoline context][trampoline context]

## Putting them all together

![2][2]

**`Consumer.scala`**

```scala
import java.time.Duration
import java.util.{Collections, Properties}

import cats.effect.{IO, Resource, Timer}
import org.apache.kafka.clients.consumer.{KafkaConsumer, ConsumerConfig => KafkaConsumerConfig}
import org.apache.kafka.common.serialization.StringDeserializer

import scala.collection.JavaConverters._
import scala.concurrent.ExecutionContext

case class ConsumerConfig(servers: String, topic: String, consumerGroup: String)

class Consumer(messageHandler: MessageHandler, config: ConsumerConfig, kafkaContext: KafkaContext) {
  implicit val timer: Timer[IO] = IO.timer(ExecutionContext.global)

  val props = new Properties()
  props.put(KafkaConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, config.servers)
  props.put(KafkaConsumerConfig.GROUP_ID_CONFIG, config.consumerGroup)

  private lazy val kafkaConsumer: KafkaConsumer[String, Array[Byte]] = {
    new KafkaConsumer[String, Array[Byte]](props, new StringDeserializer, new StringDeserializer)
  }

  def close: IO[Unit] = kafkaContext ~> kafkaConsumer.close()

  def subscribe(): IO[Unit] = kafkaContext ~> kafkaConsumer.subscribe(Collections.singletonList(config.topic))

  def consume: IO[Unit] = for {
    messages <- kafkaContext ~> kafkaConsumer.poll(Duration.ofMillis(1000)).iterator().asScala.toList
    _ <- messageHandler.handleMessage(messages)
    _ <- consume
  } yield ()

}

object Consumer {
  def resource(messageHandler: MessageHandler, config: ConsumerConfig, context: KafkaContext): Resource[IO, Consumer] =
    Resource.make(IO(new Consumer(messageHandler, config, context)))(_.close)
}
```

### Building the Consumer Application

**`ConsumerApplication.scala`**

```scala
import cats.effect.{ConcurrentEffect, ExitCode, IO, IOApp}
import scala.concurrent.{ExecutionContext, ExecutionContextExecutor}

object ConsumerApplication extends IOApp {

  implicit private val ec: ExecutionContextExecutor = ExecutionContext.global
  private val cs = IO.contextShift(ec)
  implicit private val concurrentEffect: ConcurrentEffect[IO] = IO.ioConcurrentEffect(cs)

  val config = ConsumerConfig("localhost:9092", "test-topic", "consumer-group-1")

  override def run(args: List[String]): IO[ExitCode] = {
    val applicationResource = for {
      kafkaContext <- KafkaContext.resource(cs)
      consumer <- Consumer.resource(new MessageHandler, config, kafkaContext)
    } yield consumer
    applicationResource.use(consumer => consumer.subscribe *> consumer.consume).as(ExitCode.Success)
  }

}
```

## Try it Yourself

dThis is good time to stop and try out the code we've just explained. You will build an SBT project and add dependencies of [Cats][Cats mvn], [Cats Effects][Cats Effects mvn] and [Apache Kafka][Apache Kafka mvn].

```scala
libraryDependencies ++= Seq(
  "org.typelevel" %% "cats-core" % "2.1.1"
  "org.typelevel" %% "cats-effect" % "2.1.3"
  "org.apache.kafka" % "kafka-clients" % "2.5.0"
)
```

Then, you need to add 3 files: `KafkaContext.scala`, `Consumer.scala` and `ConsumerApplication.scala` as explained above. When you finish this, you can then run `sbt compile` to check that everything is correct then you can download and run Kafka from [here][kafka download]. After downloading is done, you can extract and run Zookeeper and Kafka, create a topic and then start the Scala consumer, then you can start sending messages from Kafka console. You will need at least 4 terminals for this:

```bash
tar -xvf kafka_2.13-2.5.0.tgz
cd kafka_2.13-2.5.0

# First terminal: Start zookeeper
bin/zookeeper-server-start.sh config/zookeeper.properties

# Second terminal: Start Kafka
bin/kafka-server-start.sh config/server.properties

# Third terminal: Create a topic and then start Kafka producer console
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test-topic
bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test-topic

# Fourth terminal: Start Scala consumer
cd <your Scala code>
sbt run

# [Optional] 5th terminal: Start Kafka consumer console which can be useful for debugging
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning
```

Now, go to the third and start typing some words and press enter. Each line will be a string message and you should see them in the Scala consumer app that runs in the fourth terminal.

If you want to see the full ready-to-run code, please check out this [branch][blog branch].

# Summary

In this section, we developed a functional Kafka Scala consumer with the help of Cats and Cats Effects. We saw how we could run Kafka client in its single thread because it's not thread-safe and we saw how it's easy to switch thread pools so we can process messages in a suitable thread pool and we did this using ContextShift provided by cats library. Finally, we tested everything together including running Kafka and Zookeeper and sent messages from Kafka producer console and received them in our Scala consumer.

In [`master`][master branch] branch of the same repo, You can find more complete code that includes testing, error handling, logging, monitoring and deployment to Kubernetes. If you are interested in any of those topics, please let me know in comments and I will write about them in future posts.

![3][3]

[Apache Kafka Client]: https://kafka.apache.org/documentation/#api
[CakeSolutions client]: https://github.com/cakesolutions/scala-kafka-client/
[cakesolutions]: https://twitter.com/cakesolutions?lang=en
[David's Post]: https://medium.com/@dhpiggott/getting-started-with-kafka-using-scala-kafka-client-and-akka-f060624377a
[Alpakka project]: https://doc.akka.io/docs/alpakka-kafka/current/
[Lagom Kafka Client]: https://www.lagomframework.com/documentation/1.6.x/scala/KafkaClient.html

[Cats]: https://typelevel.org/cats/
[Cats Effect]:https://typelevel.org/cats-effect/

[Kafa client tutorial]: https://dzone.com/articles/hands-on-apache-kafka-with-scala

[Kafka Definition]: https://www.cloudkarafka.com/blog/2016-11-30-part1-kafka-for-beginners-what-is-apache-kafka.html
[pure function]: https://en.wikipedia.org/wiki/Pure_function
[cats resource]: https://typelevel.org/cats-effect/datatypes/resource.html
[thread safety]: https://typelevel.org/cats-effect/datatypes/io.html#stack-safety
[trampoline context]: https://github.com/typelevel/cats-effect/blob/v2.1.3/core/jvm/src/main/scala/cats/effect/internals/TrampolineEC.scala
[Cats mvn]: https://mvnrepository.com/artifact/org.typelevel/cats-core_2.12/2.1.1
[Cats Effects mvn]: https://mvnrepository.com/artifact/org.typelevel/cats-effect_2.12/2.1.3
[Apache Kafka mvn]: https://mvnrepository.com/artifact/org.apache.kafka/kafka-clients/2.5.0
[kafka download]: https://www.apache.org/dyn/closer.cgi?path=/kafka/2.5.0/kafka_2.12-2.5.0.tgz

[master branch]: https://github.com/dosht/kafka-with-cats/
[blog branch]: https://github.com/dosht/kafka-with-cats/tree/blog-post-code

[1]: {{ site.baseurl }}/assets/images/2020-06-27-how-functional-programming-makes-working-with-kafka-so-easy/1.png "1"
[2]: {{ site.baseurl }}/assets/images/2020-06-27-how-functional-programming-makes-working-with-kafka-so-easy/2.png "2"
[3]: {{ site.baseurl }}/assets/images/2020-06-27-how-functional-programming-makes-working-with-kafka-so-easy/3.png "3"
