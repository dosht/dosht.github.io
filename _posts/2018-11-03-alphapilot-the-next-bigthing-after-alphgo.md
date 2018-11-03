---
layout: post
title:  "AlphaPilot, the Next Big Thing After AlphGo"
author: mou
categories: [ Drones ]
image: assets/images/2018-11-03-alphapilot-the-next-bigthing-after-alphgo.png
featured: true
hidden: false
---
## AlphaGo
[AlphGo](https://deepmind.com/research/alphago/) is the first computer program to defeat a professional human [Go](https://en.wikipedia.org/wiki/Go_(game)) player, the first program to defeat a Go world champion, and arguably the strongest Go player in history. Can we achieve the same breakthrough in autonomous flying?

## FPV Drone Racing
[FPV Drone Racing](https://en.wikipedia.org/wiki/Drone_racing) is simply drone racing and FPV stands for First Person View where the pilots use virtual-reality glasses and use wireless controllers to fly the drones at high speeds above 80 MPH and they fly through a 3D course with checkpoints that pilots have to fly through as quickly as possible. It is a new emerging sport that has its own league. 

## AlphaPilot
Recently, [DRL](https://thedroneracingleague.com/) launched the Artificial Intelligence Robotic Racing (AIRR). It is the Drone Racing League’s new autonomous racing class.
[AlphaPilot](https://lockheedmartin.com/en-us/news/events/ai-innovation-challenge.html) is the first challenge  2018 - 2020. The challenge is to use AI that runs on Nvidia Jetson TX2 (and possible Jetson Xavier for 2020), so all processing will be 100% edge computing (on-drone) without offboarding data to be processed in a computer.


## The Drone Racing League Simulator
I found [DRL simulator](https://store.steampowered.com/app/641780/The_Drone_Racing_League_Simulator) in STEAM and I think it's a good start to get the feel of it and also it could be a possible way to train machine learning models in the way that DeepMind or open OpenAI use in training models to play Atari games like [this](https://gym.openai.com/envs/#atari). Those FPV racing drones simulators could be a little more difficult in integration with machine learning models if we compared with other simulators that were built originally to support research like [AirSim](https://github.com/Microsoft/AirSim). I will explore the possibility of using the racing drone simulators and write about it later.

## Research Work
A good starting point is this paper: [AirSim: High-Fidelity Visual and Physical Simulation for Autonomous Vehicles](https://arxiv.org/abs/1705.05065). It uses a convolutional neural network (CNN) with a path-planning and control system to build a  vision-based drone racing in dynamic environments. This [video](https://www.youtube.com/watch?v=8RILnqPxo1s) explains how it works.
