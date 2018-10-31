---
layout: post
title:  "Drone Simulators"
author: mou
categories: [ simulator, Drones ]
image: assets/images/drone-simulations/Simulator.PNG
featured: true
hidden: true
---


Let's explore the world of simulators and find a good that will help us most to start programming autonomous drones.
I will list a few possible alternative simulations with some pros and cons.
Some of them are built for research purposes like [AirSim](https://github.com/Microsoft/AirSim) from Microsoft
and some of them were built to train pilots of FPV racing drones like [DRL  Simulator](https://store.steampowered.com/app/641780/The_Drone_Racing_League_Simulator).
In this post I will list only simulators that were built for research, however, the other simulators can help too, but will require extra effort for integration.

## AirSim

![AirSim]({{ site.baseurl }}/assets/images/drone-simulations/AirSim.PNG)

[AirSim](https://github.com/Microsoft/AirSim) is built on Unreal Engine, open source, cross-platform and supports hardware-in-loop. It's built for AI research like deep learning, computer vision and reinforcement learning. It can generate realistic FPV video input.

Because it depends on the Unreal Engine, it's a little bit difficult to set up and requires to download a lot of stuff and also requires a powerful computer with a decent GPU.

It has got a Python and C++ [API](https://github.com/Microsoft/AirSim/blob/master/docs/apis.md)
A tiny python hello world program will look like this: [(example)](https://github.com/Microsoft/AirSim/blob/master/PythonClient/multirotor/hello_drone.py)
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

[Gezabo](https://github.com/PX4/sitl_gazebo) is a robot simulator and with [PX4 plugin](http://dev.px4.io/en/simulation/gazebo.html) [(source)](https://github.com/PX4/sitl_gazebo) you can use it for drone simulation and it can simulate FPV, but not as realistic as AirSim.
Gazebo is often used with [ROS](https://dev.px4.io/en/ros) and it has got a big community.


## Udacity QuadRotor Simulator

![UdaicytQuad]({{ site.baseurl }}/assets/images/drone-simulations/UdacityQuad.PNG)

We used the [FCND-simulator](https://github.com/udacity/FCND-Simulator-Releases/releases) while studying [FlyingCars](https://www.udacity.com/course/flying-car-nanodegree--nd787) nano degree in Udacity. It's based on Unity and supports MAVLink (and I think ROS, but I didn't try it yet!).
I used it to learn [path planning](https://github.com/dosht/FCND-Motion-Planning) and building [PID controller](https://github.com/dosht/FCND-Controls)

There is another C++ simulator that we used in the course to implement [PID Controler](https://github.com/dosht/FCND-Controls-CPP) and [EKF Estimator](https://github.com/dosht/FCND-Estimation-CPP)

[QuadRotor simulator for Controls and Deep Learning projects](https://github.com/dosht/FCND-Estimation-CPP)

[udacidrone (Python API)](https://github.com/udacity/udacidrone)

[DeepRL Quadcopter Controller](https://github.com/udacity/RL-Quadcopter)

## Udacity FixedWing Simulator

![UdaicytFixedWing]({{ site.baseurl }}/assets/images/drone-simulations/UdacityFixedWing.PNG)

This [simulator](https://github.com/udacity/FCND-FixedWing/releases) is used for the fixed wing project in FlyingCars nano degree. It's also based on Unity and uses python API. 

## V-REP


![V-REP]({{ site.baseurl }}/assets/images/drone-simulations/V-Rep.PNG)

This is a generic robot simulator that supports ROS and can be used for a drone with other kinds of robots.

[Project Link](http://coppeliarobotics.com)
