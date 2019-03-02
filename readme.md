# RL-agent to RL-agent in 1v1 Rock-Paper-Scissors game play

##  Introduction

This project is an evolution of earlier experimental project that plays an AI agent against a set of random number generators in a rock-paper-scissors game setting. The evolution is to peg two reinforcement learning agents against each other in a RPS game play.  And through this game play process, I try to understand better the underlying RL learning dynamics.   As per the base project, the agents are based on DDQN.  

You can find the reference to the base project [here](https://github.com/dennylslee/rock-paper-scissors-DeepRL).

## Evniornment set up

The overall set up of the RPS game play are shown as follow.  The two RL agents are identitical in that it uses the same RL algorithm - in this case, it is the DDQN.  DDQN is chosen since this problem is a discrete action space problem and DDQN has incorporated much of the enhancements (many contributed by DeepMind) to Q-learning and is generally considered the state-of-the-art in the dynamics programming / Q-learning brench of RL.   Besides the RL algo choice, the policy network between the two agents are also identical in that it has the same neural network architecture, the same size of network and the same set of hyperparameter settings. Furthermore, action space and reward system are also identical between the two agents.  

The only "variable" in this experiment is that the observed state space provided to the two agents are different. Player 1 will always observed the full state space of 9 dimensions.  These 9 dimensions are listed below. Player 2 will only observe a partial view of the overall state space (3 of the 9, 6 of the 9 dimensions were tested).  The intuition is that player 1 should be the stronger player since it has a wider breadth of understanding of the environment (i.e. a more sophisticated worldview); whereas player 2 should be the weaker player since it has a limited view of the game states.

![Pic1](https://github.com/dennylslee/rock-paper-scissors-RLvRL/blob/master/1v1_architecture.png)


## Game state space

There are in total nine state dimensions used in this game setting.  These are:

1. instantaneous win status
2. instantaneous tie status
3. instantaneous lost status
4. win rate trend (a binary value indicating if the moving average of win rate is going up or down)
5. tie rate trend (a binary value indicating if the moving average of tie rate is going up or down)
6. lost rate trend (a binary value indicating if the moving average of lost rate is going up or down)
7. moving average of win rate
8. moving average of tie rate
9. moving average of lost rate

## Resutls (player 2 with partial visibilty of 3 states)

The observed results for the game play in which player 2 only have state visibility of 3 (of the 9) states.  The are listed and shown in sequence below:

1. state 1, 2, 3 (i.e. player 2 only sees the instantaneous status)
2. state 4, 5, 6 (i.e. player 2 only sees the trend indicators)
3. state 7, 8, 9 (i.e. player 2 only sees the moving average rate values)

![Pic2](https://github.com/dennylslee/rock-paper-scissors-RLvRL/blob/master/Figure_1_same_NN_first3state.png)
![Pic3](https://github.com/dennylslee/rock-paper-scissors-RLvRL/blob/master/Figure_1_same_NN_mid3state.png)
![Pic4](https://github.com/dennylslee/rock-paper-scissors-RLvRL/blob/master/Figure_1_same_NN_last3state.png)

## Resutls (player 2 with partial visibilty of 6 states)

The observed results for the game play in which player 2 only have state visibility of 6 (of the 9) states.  The are listed and shown in sequence below:

1. state 1, 2, 3, 4, 5, 6 (i.e. player 2 sees instantaneous status & the trend indicators)
2. state 1, 2, 3, 7, 8, 9 (i.e. player 2 sees instantaneous status & moving average rate values)
3. state 4, 5, 6, 7, 8, 9 (i.e. player 2 sees the trend indicators & the moving average rate values)

![Pic5](https://github.com/dennylslee/rock-paper-scissors-RLvRL/blob/master/Figure_1_same_NN_first6state.png)
![Pic6](https://github.com/dennylslee/rock-paper-scissors-RLvRL/blob/master/Figure_1_same_NN_first3last3state.png)
![Pic7](https://github.com/dennylslee/rock-paper-scissors-RLvRL/blob/master/Figure_1_same_NN_last6state.png)

## Observations - surprise!

If we focus first on player 1's overall win-tie-lost rates from the above diagrams (righ hand column middle pane), it is to my surprise that player 2 does NOT appear to be a weaker player.  Firstly, player 1 (intuitively the stronger player) does not always consistently win over player 2.  There are a few marginal cases of such winning scenario but the differences (i.e. win rate over lost rate) is not that significant. Moreover there are cases in which player 2 can occasionally outperform player 1.  

... WHAT'S GOING ON? 

At the time of writing,  I am putting forward this thesis as an explanation. I postulate the following:

1. the two RL agents have equal inert learning ability since the RL algorithm is the same and the policy network design and configurations are also identical.
2. player 1 has to apply its neural capability to deal with 9 dimensions.
3. whereas player 2 has to apply its neural capability to (only) deal with a subet of the 9 dimensions.
4. my key postulation is that much of the state dimensions are somewhat a form of "distractions" which contribute little to no better understanding of the game.
5. as such, by not being distracted, it allows player 2 to sometimes plays better than player 1 (I suppose you can say that player 2 can be more "focus").

## Interesting Conjecture on the Role Reversal Dynamics

From the above diagrams, we see that both players change their move strategy from time to time.  And once changed, it tends to play that move type to the extreme (in exclusion to the other two move types).  This aspect is not entirely surprising since the feed-forward NN has no sequential or memory/attention ability, as such it just maximizes its likelihood of winning by playing the same move type over and over again.  We have seen this behaviour in previous project when a simple DNN or LSTM is played against high dimensional random generator.  

However, the more interesting question is then what causes the agent(s) to change strategy to move to another move type and how long they stay in that interim stead state? Is this change stochastic or is there something more spooky going on?  One explanation is that when one player is winning consistently, the other player is trying to find a way out via combination of exploration and utilizing the natural reward reinforcement of Q-learning.  The lossing player eventually breakthrough and fine a way out of its poor performance and become the new winning player - effectively causing a "role reversal".  And the cycle starts over again.  Now, how long the two players stay in that intermediate steady state, I conjecture, is simply a stochastic process. 

[Pics8](https://github.com/dennylslee/rock-paper-scissors-RLvRL/blob/master/p1vp2_moves.png)


