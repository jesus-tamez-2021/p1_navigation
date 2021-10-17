[//]: # (Image References)
[image2]: https://github.com/jesus-tamez-2021/p1_navigation/blob/89c5023f40f9797ea4120bf1a916f5d1f4e52eb8/image2.PNG

# Project 1: Report

All the software has been run in our local machine, with an NVIDIA GPU GeForce RTX 2070 Super, Driver version 466.77, 8GB Dedicated RAM, 8GB shared GPU Memory. The main CPU was an AMD Ryzen 7 3700X 8-Core Processor, 16GB RAM. The goal was reached in about 6 minutes.

We have used two python programs to be called by the main Jupyter Notebook Deep Q Learning program:
- dqn_agent.py - This program includes the reinforcement learning agent functions.
  - Agent Class
    - __init__ - defines two NNs (local and target), the optimizer function used on the local QNetwork (Adam), and the ReplayBuffer memory area.
    - step - saves every step in the ReplayBuffer, and each UPDATE_EVERY steps executes the learning process (only after there are enough steps in the ReplayBuffer memory (BATCH_SIZE))
    - act - for each step state, obtains and action by forwarding the local QNetwork. Deactivating the learning (Back propagation) process. And using a Greedy Policy to choose the next action.
    - learn
      - The learn process uses a sampled "experience" of BATCH_SIZE samples taken randomly from the ReplayBuffer (not using the Prioritized Experience Replay).
      - This learning process is executed less often than the steps executed on the target network (by the UPDATE_EVERY steps factor).
      - From the sampled experience, this process finds the "target Q value" by adding the memorized "reward" to the discounted (by GAMMA) "target next" Q value (obtained by applying the memorized "next state" value on the target QNetwork).
      - In addition, this function obtains the "Expecetd Q value" by using the memorized "state" on the local QNetwork, and selecting the Q Value corresponding to the memorized "action" from the ReplayBuffer memory.
      - The last part executes the local QNetwork Loss function calculation (mse_loss) based on the "Target Q Values" and the "Expected Q values", it then executes a local QNetwork back propagation, and one step of the local QNetwork optimizer function. This part updates the local Q Network parameters (weighs).
      - The final part in this function is to perform a soft_update of the target QNetwork parameters (weights).
    - soft-update
      - This process will update the local QNetwork parameters (weights) based on the Q Values (for target and local QNetworks) difference found between the two networks.
      - The target QNetwork weights are updated by adding a (TAU) proportion of the local QNetwork parameters to the (1-TAU) proportion of the target QNetwork parameters.
      - The "learn" and "soft_update" processes optimize the local QNetwork trying to reach the target QNetwork, but the target QNetwork parameters are adjusted by adding a small local QNetwork parameters contribution (TAU = 1e-3) to the target QNetwork parameters contribution defined by (1- TAU).
      - This process creates a moving target concept to avoid harmful correlations.
  - ReplayBuffer Class - experiences buffer
    - __init__ - initializes a Deque type buffer, and a named tuple (sample experience).
    - add - appends a new (step, action, reward, next step, done) tuple in the deque buffer.
    - sample - creates lists of step, action, reward, next step and done values; from the ReplayBuffer experience BATCH_SIZE random sample.
  
- model.py - this program defines the Neural Networks
  - QNetwork Class - which is based on the QNetwork class from PyTorch.
    - __init__
      - Defines the Neural Netwrok layers to be used.
      - In this case we tried several configurations, and the best results were obtained with four fully connected layers: 256, 64, 16 and 4 nodes each.
      - The last FC layer (4 nodes output) represents the four actions available in the game environment.
      - The first 3 layers are defined to create different environment "states" features being learned by the system.
    - forward
      - Builds the neural network.
      - The network created consists of non-linear RELU activation functions connecting the 4 layers.

The following output from the Jupyter Notebook execution shows the agent has been able to reach the goal of picking at least 13 yellow bananas in average for the last 100 episodes. This goal is reached in less than 500 episodes in total.

We have found that each episode has a maximum of 299 steps.

![Screenshot 1][image2]

The chart shows a linear improvement of the 10-episodes average yellow bananas picked. Some of the hyper-parameters tried produced an slow down on the learning process after 300 episodes. We tried several hyper-parameters combinations adjusting the TAU, the GAMMA, and the Epsilon parameters, in addition to the Neural Network architecture.

The best set of hyper-parameters for the learning Agent are as follows:
- BUFFER_SIZE = int(1e5)	(replay buffer size)
- BATCH_SIZE = 128		(minibatch size)
- GAMMA = 0.99		(discount factor)
- TAU = 1e-3			(for soft update of target parameters)
- LR = 5e-4			(optimizer learning rate)
- UPDATE_EVERY = 6		(how often to update the network

For the Deep Q Learning process (re-calculated every episode), the best combination was:
- epsilon start = 1.0
- epsilon end   = 0.01
- epsilon decay = 0.995

And the QNetwork(s) structure are:
- input size = 37
- fc1_units = 256, activation function = RELU
- fc2_units = 64, activation function = RELU
- fc3_units = 16, activation function = RELU
- output size = 4, activation function = RELU
- 