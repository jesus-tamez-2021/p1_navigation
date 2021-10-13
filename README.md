[//]: # (Image References)

[image1]: https://user-images.githubusercontent.com/10624937/42135619-d90f2f28-7d12-11e8-8823-82b970a54d7e.gif "Trained Agent"
[image2]: image2.png

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

The first part of the project is to setup the proper development environment for python. We are using Anaconda to create a virtual environment that includes the following packages to be imported during the jupyter notebook execution:
- random
- torch
- numpy
- pandas
- from collections import deque
- matplotlib

We included the Unity package called UnityEnvironment. Which is part of the unityagents library package installed using the pip command, as it's not avaiable on the Anaconda channels.
- from unityagents import UnityEnvironment

After several problems running the project without success to start the Unity Environment interface, we found out a dependencies guide mentioned in the project folder, as follows:
- tensorflow==1.7.1
- Pillow>=4.2.1
- matplotlib
- numpy>=1.13.3
- jupyter<6.0.0
- pytest>=3.2.2
- docopt
- pyyaml
- protobuf==3.5.2
- grpcio==1.11.0
- torch==0.4.0
- pandas
- scipy
- prompt-toolkit<3.0.0
- ipykernel

In addition, in order to be able to run and visualize the bananas environment, we have locally installed the Unity Hub 2.4.5, and installed a Unity personal license version 2020.3.18f1.

All the software has been run in our local machine, with an NVIDIA GPU GeForce RTX 2070 Super, Driver version 466.77, 8GB Dedicated RAM, 8GB shared GPU Memory. The main CPU was an AMD Ryzen 7 3700X 8-Core Processor, 16GB RAM. The goal was reached in about 6 minutes.

We have used two python programs to be called by the main Jupyter Notbook Deep Q Learning program:
- dqn_agent.py - This program includes the reinforcement learning agent functions.
  - Agent Class
    - __init__ - defines two NNs (local and target), the optimizer function used on the local QNetwork (Adam), and the ReplayBuffer memory area.
    - step - saves every step in the ReplayBuffer, and each UPDATE_EVERY steps executes the learning process (only after there are enough steps in the ReplayBuffer memory (BATCH_SIZE))
    - act - for each step state, obtains and action by forwading the local QNetwork. Deactivating the learning (Back propagation) process. And using a Greedy Policy to choose the next action.
    - learn - from the sampled exprience, finds the Q value target. By adding the memorized "reward" and the discounted (by GAMMA) "target next" Q value; obtained by applying the memorized "next state" value on the target QNetwork. In addition, obtains the Q value expected by forwading the memorized "state" on the local QNetwork, and using the memorized "action" from the ReplayBuffer memory to choose the Q value. The last part executes the Loss function calculation (mse_loss), one step of the local QNetwork optimizer function, and a soft_update of the target QNetwork weights.
    - soft-update - The target weights are updated by adding a TAU proportion of the local QNetwork parameters to the (1-TAU) proportion of the target QNetwork parameters. This process optimizes the local QNetwork trying to reach the target QNetwork, but the target QNetwork is softly adjusted with an small local QNetwork parameters contribution. TAU = 1e-3.
  - ReplayBuffer Class - experiences buffer
    - __init__ - initalizes a Deque type buffer, and a named tuple (sample experience).
    - add - appends a new (step, action, reward, next step, done) tuple in the deque buffer.
    - sample - creates lists of step, action, reward, next step and done values from the ReplayBuffer experience BATCH_SIZE random sample.
  
- model.py - this program defines the Neural Networks
  - QNetwork Class - which is based on the QNetwork class from PyTorch.
    - __init__ - Defines the Neural Netwrok layers to be used. In this case we tried several configurations, and the best results wre obtained with four fully connected layers: 256, 64, 16 and 4 nodes each. The last FC layer (4 nodes output) represents the four actions available in the game environment. The first 3 layers are defined to create different environment "states" features being learned by the system.
    - forward - Builds the neural network. The network created consists on non-linear RELU activation funtions connecting the 4 layers.

The following output from the Jupyter Notbook execution shows the agent has been able to reach the goal of picking at least 13 yellow bananas in average for the last 100 episodes. This goal is reached in less than 500 episodes in total. We have found that each episode has a maximum of 299 steps.

![Exmaple 1][image2]

The chart shows a linear improvement of the 10-episodes average yellow bananas picked. Some of the hyper-parameters tried produced an slow down on the learning process after 300 episodes. We tried several hyper-parameters combinations adjusting the TAU, the GAMMA, and the Epsilon parameters; in addition to the Neural Network architecture.

The best set of hyper-paramteres for the learning Agent are as follows:
- BUFFER_SIZE = int(1e5)    ( replay buffer size )
- BATCH_SIZE = 128          ( minibatch size )
- GAMMA = 0.99              ( discount factor )
- TAU = 1e-3                ( for soft update of target parameters )
- LR = 5e-4                 ( optimizer learning rate )
- UPDATE_EVERY = 6          ( how often to update the network )

For the Deep Q Learning process (re-calculated every episode), the best combination was:
- epsilon start = 1.0
- epsilon end   = 0.01
- epsilon decay = 0.995

And the QNetwork(s) structure are:
- input size = 37
- fc1_units = 256,  activation function = RELU
- fc2_units = 64,   activation function = RELU
- fc3_units = 16,   activation function = RELU
- output size = 4,  activation function = RELU

### Instructions

Create an Anaconda environment. Choose to use Python 3.6

Install all the program dependencies mentioned in the previous paragraphs. Using the pip command you can specify the package version to be installed (==, <, >=, etc.). If the packages has been installed through Anaconda, you can pull down the package version number and choose a differnt one to be used in the environment. If you have a different version installed by mistake, you can use pip to unistall that version and the run pip again to install the right one.

We ran the Jupyter notebook from within VS Code. With the following steps:
- Clone the repository with Git.
- Use (or install) VS Code 1.61
- Open the local (downloaded) folder in the VS Code explorer window.
- Select the JT_Navigation.ipynb file.
- Execute each one of the cells, or all at once.
- If the Unity Environment is properly installed, a Unity window will be opened and you can see the agent collecting the yellow bananas, and learning in each episode. Depending on the local commpter configuration, this collection process could be executed very fast and difficult to appreciate for the human eye.


