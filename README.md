[//]: # (Image References)
[image1]: https://user-images.githubusercontent.com/10624937/42135619-d90f2f28-7d12-11e8-8823-82b970a54d7e.gif "Trained Agent"
[image2]: https://github.com/jesus-tamez-2021/p1_navigation/blob/89c5023f40f9797ea4120bf1a916f5d1f4e52eb8/image2.PNG

# Project 1: Navigation

### Introduction
For this project, you will train an agent to navigate (and collect bananas!) in a large, square world. 

![Trained Agent][image1]

A reward of +1 is provided for collecting a yellow banana, and a reward of -1 is provided for collecting a blue banana.  Thus, the goal of your agent is to collect as many yellow bananas as possible while avoiding blue bananas. 

The state space has 37 dimensions and contains the agent's velocity, along with ray-based perception of objects around agent's forward direction.  Given this information, the agent must learn how to best select actions.  Four discrete actions are available, corresponding to:
- **`0`** - move forward.
- **`1`** - move backward.
- **`2`** - turn left.
- **`3`** - turn right.

The task is episodic, and in order to solve the environment, your agent must get an average score of +13 over 100 consecutive episodes.

### Getting Started

The first part of the project is to setup the proper development environment for python. We are using Anaconda to create a virtual environment that includes the following packages to be imported during the jupyter notebook execution:
- torch
- numpy
- pandas
- matplotlib
  
We included the Unity package called UnityEnvironment. Which is part of the unityagents library package installed using the pip command, as it's not available on the Anaconda channels.
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

### Installation Instructions

Create an Anaconda environment; choosing to use Python 3.6.

Install all the program dependencies mentioned in the previous paragraphs.
- Using the pip command, you can specify the package version to be installed (==, <, >=, etc.).
- If the packages have been installed through Anaconda, you can pull down the package version number and choose a different one to be used in the environment.
- If you have installed a different version using the pip command, you can use pip to uninstall that version and the run pip again to install the right version required.

We ran the Jupyter notebook from within VS Code. With the following steps:
- Clone the repository with Git.
- Use (or install) VS Code 1.61
- Open the local (downloaded) folder in the VS Code explorer window.
- Select the Navigation.ipynb file.
- Execute each one of the cells, or all at once.
- If the Unity Environment is properly installed, a Unity window will be opened, and you can see the agent collecting the yellow bananas and learning in each episode. Depending on the local computer configuration, this collection process could be executed very fast and difficult to appreciate for the human eye.
