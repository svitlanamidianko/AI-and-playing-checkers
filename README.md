# AI-and-playing-checkers
Repo for the minimax and alpha-beta pruning algorithm that plays checkers with you. See the pdf for described strategy.
Developed by @svitlanamidianko.

# 1. Problem Definition

A checkerboard game *Checkers* has been active for over 5000 years and gained popularity in multiple parts of the world, including Ukraine, where I used to play this game as a child. *Checkers* is a zero-sum game played by two players and involves a high degree of strategic and tactical thinking. Even though there are many variations of the game, the standard version of it is described below.

The game's initial state consists of $12\times 2$ checkers placed on opposite sides of $8\times 8$ checkerboard in a checker-like manner. Each player starts with the symmetric position and takes turns alternating.

While playing, the **players are allowed to:**

1. make diagonal moves of uniform game pieces;
1. make mandatory captures by jumping over opponent pieces.

And the **rules of the game are as follows:**

- a. Capture is possible only in the forward direction. The exception is when the capture is a part of multiple stages captures on the same move, then the capture is possible also backward.
- b. When a checker reaches the last line, it is crowned to Queen (in the Ukrainian version; in the standard version, it is often named King). Queens can move both backward and forward, and they're also able to capture both backward and forward.

The **player's goal** is to eliminate all the opponent's checkers or surround the remaining checkers, which determines a victory. 

*Checkers* is a *competitive multi-agent* environment. It is also *deterministic,* meaning that the $state_{i+1}$ of the game/environment is fully determined by the $state_i$  and action executed by the player. The state of the game is *static* so that it cannot change while the player is deliberating (if the game is played without a clock). It is also *fully observable* for players to see what opponents' and their state of checkers are. The game is also *sequential*, so the player's best move will depend on the opponent's previous move. Because there is a set of actions, this game also has a *discrete* environment. The search space of the game is enormous — around $10^{20}$, but not as big as in chess (~ $`10^{120}`$ ). Branching factor is $`2.8`$ .

# 2. Solution Specification

To solve this problem, I developed this game in Python. Using this code, it is possible to:  
- (1) watch two AIs playing against each other; 
- (2) play against chosen AI. There are multiple configurations of AI that can be tried out, such as AI using solely minimax, AI using minimax and alpha-beta pruning, AI using either one of the two evaluation functions. 

The `MiniMax` algorithm is an appropriate choice of algorithm for this game. It relies on having zero-sum, opposite interests between the players — each of them wanting to win. `MiniMax` constructs the tree branching out of each possible board state, starting from the root node (current state).  On each level of the tree, the goal is to either maximize or minimize the value of the evaluation function. This is because, assuming that the opponent plays smart, we know that the opponent wants to minimize the score, which would happen every other move. It is a recursive algorithm, so it repeats for each of the states of the board, resulting in exponential growth with extra additional depth level. See figure 1 for visualization.

![image](https://user-images.githubusercontent.com/44281687/121323786-cb2c2c80-c918-11eb-87d6-8db892ca7b8c.png)

_Figure 1. Visualization of the MiniMax algorithm for C*heckers*. On each of the depth the goal is to either maximize or minimize value of the evaluation function. With alpha-beta pruning number of evaluations decreases because of disregard of the states that wouldn't improve the player's score.

One of the weaknesses of such an algorithm is its almost brute-like structure, where the player/AI has to check *all* possible movements and evaluate what their outcomes are. This requires a lot of computation power. To optimize this, I also implemented alpha-beta pruning, which stores two extra values of $\alpha, \beta$ and prunes away the branches to minimize the number of `MiniMax` evaluations. Essentially, on each level of the tree, the algorithm stops evaluating branches on that depth of the tree once there was at least one possibility found that proves the next move to be worse than a previously examined move. It returns the same result as simple `MiniMax` but prunes away the branches on the way hence minimizing the number of minimax evaluations needed.

Figure 2 displays the diagram of the classes and functions implemented to make it work. Since the evaluation of the whole tree is not possible, I limit the depth of the tree.

![image](https://user-images.githubusercontent.com/44281687/121323837-d54e2b00-c918-11eb-9985-aa1816c66ba2.png)

_Figure 2. Diagram of the classes and functions implemented for the checkers game. The tables manifest the classes, its attributes and methods. The red modes manifest the functions that the user can call at first. Next, two versions of MiniMax functions (with and without alpha-beta pruning) are implemented (in grey). They rely upon either one of the evaluation functions (in blue).  See image in better resolution [here](https://whimsical.com/cs152-fp-4soGDMtPZ84XKEGgFiJS61)._
```
