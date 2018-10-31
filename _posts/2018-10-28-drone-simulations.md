---
layout: post
title:  "Drone Simulators"
author: mou
categories: [ simulator, Drones ]
image: assets/images/drone-simulations/Simulator.png
featured: true
hidden: true
---

Let's explore the world of simulators and find a good that will help us most to start programming autonmous drones.
I will list a few possible alternative simulations with some pros and cons.
Some of them are build for research purposes like [AirSim](https://github.com/Microsoft/AirSim) from Microsoft
and some of them were build to train pilots of FPV racing drones like [DRL  Simulator](https://store.steampowered.com/app/641780/The_Drone_Racing_League_Simulator).
In this post I will list only simulators that were build for research, however, the other simulators can help too, but will require extra effort for integration.

## AirSim

![AirSim]({{ site.baseurl }}/assets/images/drone-simulations/AirSim.PNG)

[AirSim](https://github.com/Microsoft/AirSim) is built on Unreal Engine, open source, cross-platform and supports hardware-in-loop. It's build for AI research like deeplearning, computer vision and reinforcement learning. It can generate realistic FPV video input.

Because it depends on Unreal Engine, it's a little bit difficult to setup and requires to download a lot of stuff and also requires a powerful computer with a descent GPU.

It has got a Python and C++ [API](https://github.com/Microsoft/AirSim/blob/master/docs/apis.md)
A tiny python hello world program will look like this: [(exmple)](https://github.com/Microsoft/AirSim/blob/master/PythonClient/multirotor/hello_drone.py)
```python
import airsim

# connect to the AirSim simulator 
client = airsim.MultirotorClient()
client.confirmConnection()
client.enableApiControl(True)
client.armDisarm(True)

# Async methods returns Future. Call join() to wait for task to complete.
client.takeoffAsync().join()
client.moveToPositionAsync(-100, 100, -100, 50).join()
```

## Gazebo

![Gazebo]({{ site.baseurl }}/assets/images/drone-simulations/Gazebo.PNG)

[https://github.com/PX4/sitl_gazebo](Gezabo) is a robot simulator and with [PX4 plugin](http://dev.px4.io/en/simulation/gazebo.html) [(source)](https://github.com/PX4/sitl_gazebo) you can use it for drone simulation and it can simulate FPV, but not as realistic as AirSim.
Gazebo is often used with [ROS](https://dev.px4.io/en/ros) and it has got a big community.

http://dev.px4.io.s3-website-us-east-1.amazonaws.com/simulation-gazebo.html


## Udacity QuadRotor Simulator

![UdaicytQuad]({{ site.baseurl }}/assets/images/drone-simulations/UdacityQuad.PNG)

https://github.com/udacity/RoboND-QuadRotor-Unity-Simulator

https://github.com/udacity/udacidrone

https://github.com/udacity/RL-Quadcopter

## Udacity FixedWing Simulator

![UdaicytFixedWing]({{ site.baseurl }}/assets/images/drone-simulations/UdacityFixedWing.PNG)

## V-REP

![V-REP]({{ site.baseurl }}/assets/images/drone-simulations/V-Rep.PNG)

http://coppeliarobotics.com
