---
layout: post
title:  "Learning Computer Vision (Machine Learning)"
author: mou
categories: [ AI, ComputerVision, ML ]
image: assets/images/2019-08-22-learning-computer-vision-machine-learning.jpg
featured: true
hidden: false
sitemap: true
---

The Computer Vision (CV) is simply teaching a computer how to see and understand the content of images and videos. We are using already many applications of CV on our phones and computer. For example, when you post a picture on Facebook of you and your friends, Facebook then uses CV to detect faces in the picture and then suggest who those friends could be. Also in Google Photos, you can search in your picture for cats and it will show you pictures that contain cats even if the name of the picture doesn’t contain the word cat.


In this post, I will explain one possible learning path to study computer vision through machine learning. The suggestions below are biased by my experience and a bit opinionated, so I discarded a lot of similar alternatives and listed only what I think a short path, but good enough to get started. I also will not mention anything about traditional computer vision methods that use OpenCV which is also very important, but it 'will make the post too long.

If you want to build something today to get an idea of what you are going to do, please check out this [quick tutorial](https://www.tensorflow.org/beta/tutorials/keras/basic_classification)

## 1. Machine Learning >> [link](https://www.coursera.org/learn/machine-learning)
Now let’s start from bottom up. The first course is the machine learning by Andrew Ng. ml-class. Andrew Ng. is a co-founder of Coursera and the ml-class was one of the first 3 courses in Coursera even before Coursera itself started. You can think about it as ML-101 because this course will set all the basics you need to understand any advanced machine learning topic. It will give the intuition of what and how machine learning works and important concepts like what model training is, cross-validation, overfitting and hyperparameter tuning, ..etc. It also covers a broad area of machine learning types. It starts from a simple version of linear regression algorithm and takes you up to the simplest version of neural networks - which will be your first step in the following course.

_Tip_: In this course don’t be too fast and try to understand every concept and take your time to get the intuition of the model training of the simple linear regression at the beginning of the course, so draw the graphs and understand the math and implement every part yourself.

_Tip 2_: Don’t spend too long to learn Octave (the programming language used in the course) because probably you will not use it again!

You might need some pre-requisites in Calculus, Linear Algebra and Probabilities, but the course itself will redirect you to resources to learn from at the beginning.

Checkpoint: From now on, you will need to know Python and you will write more code. Whether you already know Python or not, I suggest going throw this course [MIT CS-101](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-00-introduction-to-computer-science-and-programming-fall-2008/). It will not only teach you Python but also and more important will make a good developer. I’m using concepts I learned from this course almost on a daily bases since I first took it more than 5 years ago.

## 2. Deep Learning >> [link](https://www.coursera.org/learn/neural-networks-deep-learning)

The second course is about deep learning and the main instructor is also Andrew Ng. It will expand what you studied in the Machine Learning about artificial neural networks, but with more details. It will be a good idea to start using one of the most famous machine learning libraries like [Tensorflow](https://www.tensorflow.org) and [Keras](https://keras.io) and you can combine both in this [tutorial](https://www.tensorflow.org/tutorials/keras/basic_classification). This course is also the first course in the [Deep Learning Specialisation](https://www.coursera.org/specializations/deep-learning) in Coursera which is a set of 5 courses and if you want to finish all of them, I suggest to take them in this order (1 - 4 - 5 - 2 - 3). You may or may not like that idea, but it’s just my suggestion.

(Optional) Deep Learning by Geoffrey Hinton one of the heroes of deep learning [(link)](https://www.cs.toronto.edu/~hinton/coursera_lectures.html). The course is now somehow outdated, so I don’t suggest to go throw all of it, but it still gives a very good theoretical introduction to deep learning. I suggest you watch the lectures with only a paper and a pencil until the video (7e - Long term short term memory)

(Shortcut) If you are more a self-learner and like like the hard way to learn things, then you can read Part II of the Deep Learning book [(link)](https://www.deeplearningbook.org) and skip the last course. You should apply each model with Tensorflow and/or Keras and expect that some parts will not be so easy to understand and you will need to google and read some tutorials and watch some videos until you get it done.

## 3. Convolutional Neural Network >> [link](https://www.coursera.org/learn/convolutional-neural-networks) 
So the third and last course in our learning path is Convolutional Neural Netrowk and this the most common model that is used in Computer Vision. During studying the course, This course is the 4th course in the deep learning specialization. And now, I would recommend starting to apply the concepts you learn into something more practical. You can search for some good datasets that you can use to apply your knowledge and there is a famous website for data science competitions called [Kaggle](https://www.kaggle.com) where you start with a dataset and an objective and try to find the best machine learning model. You can also check out Google colab; It’s a web-based environment that enables you to write code and run experiments on Google cloud for free!

Note: One important model that I couldn’t find (yet) in courses is called [Generative Adversarial Network (GANs)](https://skymind.ai/wiki/generative-adversarial-network-gan).
![image](/assets/images/2019-08-22-mona.gif)

## Practising

The last and most exciting step(s) is to read research papers and pick one of them and try to implement it yourself and see if you can get similar results. You can pick your favourite paper from [papers-with-code](https://paperswithcode.com) First try to find an interesting paper and implement it yourself then if/when you stuck, look at the code. If you get inspired by one of those papers, you can try to implement an application that uses that model and maybe start your startup :D 

_Tip_: There are too many papers and you can spend weeks implementing one paper, so before you pick one, scan many papers by reading the abstract and conclusion and maybe a couple of sections that could be relevant and then decide to go further or skip to another paper.

I hope after you read this article, you now have a better idea of how you can self-study computer vision and if you have any question, please ask the comments section below.

Enjoy and see you again in 6 months!
