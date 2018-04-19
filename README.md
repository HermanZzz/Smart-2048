# Smart-2048
**Solving the game 2048 with different AI algorithms**

### Introduction

2048 is a game featuring a 4x4 grid-board with numbered tiles. In each turn, a new tile with either 2 or 4 appears randomly in an empty slot. The player can then slide all tiles in onedirection (NWSE) until they hit another numbered tile or the wall of the board. Two tiles withequal values will merge upon collision, and the goal is to get a 2048 tile before all slots are filled.

In this project we would like to explore and compare different algorithms to solve the 2048 game.

### Methodology

As 2048 has a discrete state space and is fully observable and stochastic, we are considering atleast 3 possible algorithms to solve this puzzle:

1. Minimax
2. Expectimax
3. Deep reinforcement learning

#### 1. Minimax

We consider Minimax as 2048 can be modeled as a game between two agents, with the playermoving the tiles while the computer placing random tiles. Although, unlike traditional boardgames, players take drastically different actions in 2048, adversarial search can still be appliedif, instead of taking random actions, the computer places tiles to minimize the player’s chance of winning.

#### 2. Expectimax

Alternatively, expectimax can be used if we can find out the probability of the computer

- spawning 2 or 4 (trivial), and/or
- filling in a certain empty slot (non-trivial)

The algorithm will generate 4 * 2 * n nodes for each layer, because there are 4 possible moves,2 numbers for the spawned tile in n empty slots.

**Heuristics**

In both Minimax and Expectimax, we will come up with heuristics to evaluate a given state of the game. Here is a preliminary list of heuristics and the intuitions behind:

- Number of ordered tiles
  - ensure tiles will not block each other from merging
- Number of empty slots
  - the more the empty slots, the less likely the game ends prematurely
  - on the flip side, it means more randomness for the location of the spawned tile
- Higher values in the corners
  - allow easier ordering of tiles
- Number of immediate merges from the move
  - ‘greedy’ heuristics that prevents us from losing
- Number of potential merges after the move
  - ‘delayed’ satisfaction resulting from delayed movement
  - balance the impact of greedy approach
  - measured by number of adjacent tiles with equal value

The evaluation function will weigh these heuristics and generate a score for every state of thegame.

#### 3. Deep reinforcement learning

Some literature suggest that deep reinforcement learning could also be used to solve games like 2048, in which the Bellman equation is used iteratively to learn the optimal value function.We hope to implement this with neural network or linear function as the function approximator.The heuristics mentioned above can be reused to define the reward function.

### Demo

##### Environment

```
C++ compiler
Python 2.7
Google Chrome 64.0
```

##### Execute C++

```
>>> cd Smart-2048
>>> ./configure
>>> make
```

##### Run the demo

1. **Run in the termial**

   ```
   >>> python bin/2048
   ```


2. **Run in the Chrome**

- Open Chrome remote debugging model 

  ```
  >>> Google\ Chrome --remote-debugging-port=9222
  ```

- Open any [2048](http://2048game.com/) in the Chrome

- Run the demo

  ```
  >>> python 2048.py -b chrome
  ```

**Demo screenshot**

![screen1](https://raw.githubusercontent.com/HermanZzz/Smart-2048/master/img/screen1.png)

