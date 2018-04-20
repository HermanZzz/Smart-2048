# Smart-2048
**Solving the game 2048 with AI**

### Introduction

2048 is a game featuring a 4x4 grid-board with numbered tiles. In each turn, a new tile with either '2' or '4' appears randomly in an empty slot. The player can then slide all tiles in onedirection (NWSE) until they hit another numbered tile or the wall of the board. Two tiles withequal values will merge upon collision, and the goal is to get a 2048 tile before all slots are filled.

![2048](https://raw.githubusercontent.com/HermanZzz/Smart-2048/master/img/2048.png)
### Algorithm

In computer science terminologies, this game is fully observable, stochastic and has a discrete state space of 2^64. So we are considering some possible algorithms to solve this puzzle:

1. Minimax
2. Expectimax
3. Deep reinforcement learning

#### 1. Minimax

We consider Minimax as 2048 can be modeled as a game between two agents, with the playermoving the tiles while the computer placing random tiles. Although, unlike traditional boardgames, players take drastically different actions in 2048, adversarial search can still be appliedif, instead of taking random actions, the computer places tiles to minimize the player’s chance of winning.

#### 2. Expectimax

We found the exact probability of '2' and '4' appearing in the empty square. So under expectimax, we use a tree search algo to explore different game states, and pick the move that results in the highests expected score. The algorithm will generate 4 * 2 * n nodes for each layer, because there are 4 possible moves, 2 numbers for the spawned tile in n empty slots. The performance heavily depends on how the heuristics are generated.

#### 3. Deep reinforcement learning

Some literature suggest that deep reinforcement learning could also be used to solve games like 2048, in which the Bellman equation is used iteratively to learn the optimal value function.We hope to implement this with neural network or linear function as the function approximator.The heuristics mentioned above can be reused to define the reward function.

#### Heuristics

In both Minimax and Expectimax, we will come up with heuristics to evaluate a given state of the game. Here is a list of heuristics and the intuitions behind:

1. Number of empty squares;

- The more the empty slots, the less likely the game ends prematurely.
- On the flip side, it means more randomness for the location of the spawned tile.
- Tend to merge the tiles to free up empty squares - greedy approach.

2. Number of immediate merges from the move;

- The number of adjacent tiles with equal values. 
- The higher the better.

3. Sum of all values on the board;

- To reduce the chance of forming a big numbers prematurely before lining up the board properly. 
- To delay merges which further neutralizes the effect of the first heuristic.
- The lower the better.

4. The smoothness of the board

- Number of ordered tiles.
- Ensure tiles will not block each other from merging

5. Highest values in the corners

- Allow easier ordering of tiles

The evaluation function will weigh these heuristics and generate a score for every state of thegame.

### Demo

##### Environment

``` bash
- C++ compiler
- Python 2.7
- Google Chrome 43.0
```

#### Run the demo

**Execute C++**

```bash
>>> cd Smart-2048
>>> ./configure
>>> make
```

1. **Run in the termial**

   ``` bash
   >>> python bin/2048
   ```



2. **Run in the Chrome**

- Open the Chrome remote debugging model.

  ``` bash
  >>> Google\ Chrome --remote-debugging-port=9222
  ```

- Open any [2048](http://2048game.com/) in the Chrome, which is controlled by keyboard arrow keys.

- Run the python file and enjoy it in the webpage.

  ``` bash 
  >>> python 2048.py -b chrome
  ```

**Demo screenshot**

![screen1](https://raw.githubusercontent.com/HermanZzz/Smart-2048/master/img/screen1.png)

3. **Run the configuration model**

  We make a a script for testing the algorithm lots of times with different parameters.You can edit it and change some parameters such as the depth limit and the iteration size.The result will be saved in a txt format and the file name is specified in the main function.After finishing, you could get some statistics from the result.

  1. Rename '2048Test.cpp' -> '2048.cpp'
  2. Execute and run

      ``` bash
      >>> make
      >>> bin/2048
      ```

  1. Get result


### Implementation

##### Bitboard

We use bit board to represent the game. In the bitboard representation format, 2 is used to represent '4' in the board, 7 is used to represent '128', etc.

- The index of the number - 4-bit nibble
- One row/column - unsigned 16 bit integer.
- The whole game board - unsigned 64 bit integer.

##### Mapping table

We use a mapping table on rows to generate new game board under current board and a move.

- Consider all possible states of a row 
- Generate result for a move to the left (up, down, right is just different rotation)

As a result, we can perform bitwise operation to extract all rows and columns. So as to speed up preparation of board for mapping.

##### Prune the search tree

It is still impossible to explore every state. So we do the following improvements to prune the search tree:

- Dynamic depth limits
- Prunes trees with low probability
- Maintain a transposition table

### Video explanation

If you're interested, please enjoy more in our [YouTube video](https://youtu.be/95Y5eVkuGiU)

### Reference

1. [Play 2048 Game Online](http://gabrielecirulli.github.io/2048/)
2. [Robert Xiao. AI for the 2048 game](https://github.com/nneonneo/2048-ai)
3. [Randy Olson. Artificial Intelligence has crushed all human records in 2048](http://spartanideas.msu.edu/2015/04/27/artificial-intelligence-has-crushed-all-human-records-in-2048-heres-how-the-ai-pulled-it-off/)
4. [Kuang-che Wu. 2048 puzzle AI module](https://github.com/kcwu/2048-python)
5. [Jiri Novotny. MI-MVI_2016](https://github.com/gorgitko/MI-MVI_2016)
6. [Connor, Byron. Artificial intelligence for 2048 game](https://github.com/rcbyron/2048-ai)
7. [Adam Szczepański. Mastering-2048](https://github.com/aszczepanski/2048)
8. [Yiyuan Lee. 2048 AI – The Intelligent Bot](https://codemyroad.wordpress.com/2014/05/14/2048-ai-the-intelligent-bot/)
9. [Sandipan Dey. Using Minimax with Alpha-Beta Pruning and Heuristic Evaluation Functions to Solve the 2048 game with a Computer](https://sandipanweb.wordpress.com/2017/03/06/using-minimax-with-alpha-beta-pruning-and-heuristic-evaluation-to-solve-2048-game-with-computer/)

