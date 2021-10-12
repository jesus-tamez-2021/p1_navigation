[//]: # (Image References)

[image1]: https://user-images.githubusercontent.com/10624937/42135619-d90f2f28-7d12-11e8-8823-82b970a54d7e.gif "Trained Agent"
[image2]: /image2.png

# Project 1: Navigation

### Introduction

For this project, you will train an agent to navigate (and collect bananas!) in a large, square world.  

![Trained Agent][image1]

A reward of +1 is provided for collecting a yellow banana, and a reward of -1 is provided for collecting a blue banana.  Thus, the goal of your agent is to collect as many yellow bananas as possible while avoiding blue bananas.  

The state space has 37 dimensions and contains the agent's velocity, along with ray-based perception of objects around agent's forward direction.  Given this information, the agent has to learn how to best select actions.  Four discrete actions are available, corresponding to:
- **`0`** - move forward.
- **`1`** - move backward.
- **`2`** - turn left.
- **`3`** - turn right.

The task is episodic, and in order to solve the environment, your agent must get an average score of +13 over 100 consecutive episodes.

### Getting Started

The first part of the project is to setup the proper development environment for python. We are using Anaconda to create a virtual environment that includes the following packages:
- random
- torch
- numpy
- pandas
- from collections import deque
- matplotlib

We have created two python programs to called by the main Jupyter Notbook program:
- dqn_agent
- unityagents

To be able to run and visualize the bananas environment, we have locally installed the unity hub 2.4.5, and installed a Unity personal license version 2020.3.18f1.

![Exmaple 1][image2]

### Instructions

Follow the instructions in `Navigation.ipynb` to get started with training your own agent!  

