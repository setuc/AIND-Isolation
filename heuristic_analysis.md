# Heuristic Analysis

As mentioned in the lecture videos that a complete search is impossible making the search of the set of moves very difficult.
Hence to augment the search we would like to implement some heurisitics to be able to determine if the sub-tree is worth the
evaluation.
 
Based on the code that was available <code><span style="color:red">improved_score.py </span></code> I experimented with various 
logic built around it. My understanding of the scoring logic was the number of moves for the player and opponent  is equivalent
to the degrees of freedom available at each step. Doing a subtraction of the degress of freedom of the opponent from that of the
player gives the degrees of freedom that is advantages to the player. This is the number that we need to maximize, i.e. higher 
number corresponds to a better scenario for the player over that of the opponent. 

It is important to note that the starting position of the values are based on random starting position and response. 

<code>

    for _ in range(2):
        move = random.choice(games[0].get_legal_moves())
        games[0].apply_move(move)
        games[1].apply_move(move)
</code>

Hence the comparision of runs are not statistically valid due to the randomness. There are two potential options we have 
to mitigate this issue:
1. Carry out sufficiently large number contests
2. Fix it with a seed <code> random.seed(1334) </code>. Though it wouldn't matter as the submission is only for the game_agent.py 

This also might not be relevant for the tournament as the search would then only rely only on the scoring heuristics. 

## Simple factor based analysis. 

My approach was simple, I tried to play with the multiplication factor as given in this equation

<code> score = player_moves - <bold>factor</bold> X opponent_moves </code>

Table of Factors

| Opponent    | F=1                                                            | F=3                                                            | F=10                                                           |
|-------------|----------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|
| Random      | 20                                                             | 20                                                             | 19                                                             |
| MM_Null     | 20                                                             | 19                                                             | 19                                                             |
| MM_Open     | 20                                                             | 18                                                             | 20                                                             |
| MM_Improved | 19                                                             | 19                                                             | 20                                                             |
| AB_Null     | 19                                                             | 20                                                             | 19                                                             |
| AB_Open     | 20                                                             | 19                                                             | 19                                                             |
| AB_Improved | 18                                                             | 19                                                             | 20                                                             |
| Total       | 97.14%                                                         | 95.71%                                                         | 97.14%                                                         |
| Reference   | [1](https://gist.github.com/setuc/882a2e22ef43215a4103b866525a1ae9) | [2](https://gist.github.com/setuc/c0560d1b3dffca3b652b2f329568176c) | [3](https://gist.github.com/setuc/1d7570d7546735769cf0669664810edb) |
| Baseline    | 97.86%                                                         | 95.71%                                                         | 97.14%                                                         |

There is no improvement in the scoring as its is based off the improvement logic that it is already competing with. Though its 
strange that adding a multiplication factor still did not result in changes on the agreesiveness of the scoring logic. 

## Difference in moves *MULTIPLIED or DIVIDED* by number of free cells

This approach was tried to introduce a bit of dynamics based on the number of free cells available to increase/decreasae
the aggresiveness of the algorithm. In the beginning, the factor will be higher/lower which will keep reducing/increasing 
as the game progresses.  

### Division

| Opponent    | F=1                                                            | F=3                                                            | F=10                                                           |
|-------------|----------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|
| Random      | 39                                                             | 37                                                             | 37                                                             |
| MM_Null     | 37                                                             | 37                                                             | 34                                                             |
| MM_Open     | 36                                                             | 38                                                             | 38                                                             |
| MM_Improved | 39                                                             | 37                                                             | 36                                                             |
| AB_Null     | 40                                                             | 40                                                             | 39                                                             |
| AB_Open     | 35                                                             | 36                                                             | 37                                                             |
| AB_Improved | 39                                                             | 38                                                             | 39                                                             |
| Total       | 94.64%                                                         | 93.93%                                                         | 92.86%                                                         |
| Reference   | [4](https://gist.github.com/setuc/c9e443afb67e4dc3bda0055785d35ed3) | [5](https://gist.github.com/setuc/5b3151b458a90cb9fc7ccb6fcb576423) | [6](https://gist.github.com/setuc/cae5b6b4b25df67227f7413ad7438b2c) |
| Baseline    | 96.07%                                                         | 95.71%                                                         | 95.00%                                                         |

The division by the number of free cells actually decreases the performance of the search. 

### Multiplication

| Opponent    | F=3                                                            | F=5                                                            | F=10                                                           |
|-------------|----------------------------------------------------------------|----------------------------------------------------------------|----------------------------------------------------------------|
| Random      | 20                                                             | 39                                                             | 36                                                             |
| MM_Null     | 19                                                             | 39                                                             | 39                                                             |
| MM_Open     | 19                                                             | 40                                                             | 37                                                             |
| MM_Improved | 19                                                             | 38                                                             | 37                                                             |
| AB_Null     | 20                                                             | 39                                                             | 37                                                             |
| AB_Open     | 19                                                             | 34                                                             | 39                                                             |
| AB_Improved | 19                                                             | 38                                                             | 37                                                             |
| Total       | 96.43%                                                         | 95.36%                                                         | 93.57%                                                         |
| Reference   | [7](https://gist.github.com/setuc/11afc1db538a3e641a2dbb047a107e23) | [8](https://gist.github.com/setuc/43d0d789668f137c8e1128d2391a2e6e) | [9](https://gist.github.com/setuc/396391e4ecf68097cdd23310f8c66d01) |
| Baseline    | 95.00%                                                         | 93.93%                                                         | 95.36%                                                         |

This was the only logic that resulted in the best score among the analysis. A further fine turning using any black box 
optimization techniques, like bayesian optimization will result in an optimum value. I will continue to find a simple
logic and improved heurisitics to determine the final number. 

### Multiplication by the number of faction of the number of moves(N)

| Opponent    | N/1                                                              | N/2                                                              | N/5                                                              | N/10                                                             | N/15                                                             |
|-------------|------------------------------------------------------------------|------------------------------------------------------------------|------------------------------------------------------------------|------------------------------------------------------------------|------------------------------------------------------------------|
| Random      | 35                                                               | 34                                                               | 38                                                               | 39                                                               | 36                                                               |
| MM_Null     | 38                                                               | 36                                                               | 39                                                               | 39                                                               | 38                                                               |
| MM_Open     | 30                                                               | 38                                                               | 38                                                               | 35                                                               | 37                                                               |
| MM_Improved | 32                                                               | 39                                                               | 37                                                               | 38                                                               | 37                                                               |
| AB_Null     | 34                                                               | 37                                                               | 38                                                               | 39                                                               | 35                                                               |
| AB_Open     | 34                                                               | 36                                                               | 39                                                               | 37                                                               | 39                                                               |
| AB_Improved | 34                                                               | 39                                                               | 36                                                               | 38                                                               | 36                                                               |
| Total       | 84.64%                                                           | 92.50%                                                           | 94.64%                                                           | 94.64%                                                           | 92.14%                                                           |
| Reference   | [10](https://gist.github.com/setuc/5f500bcaa844a3664ae394d194145dd7) | [11](https://gist.github.com/setuc/1ba6549a613732420a09e6f5803dd0ba) | [13](https://gist.github.com/setuc/41fb5c236b78fdb1cad04ecbd7e9a5da) | [14](https://gist.github.com/setuc/78046712e1dd7c0f6f62935b1d2d2fdf) | [15](https://gist.github.com/setuc/08dce9f61216dc1a0d628718595debf7) |
| Baseline    | 96.07%                                                           | 95.71%                                                           | 93.57%                                                           | 94.29%                                                           | 97.14%                                                           |

## Look 1 ply ahead logic
In this approach, I took a sum of all legal moves from the current moves that are available. The similar difference of 
the player vs oppenent moves followed in the logic. The results were a order of magnitude different than the baseline
score and hence did not purse further. See [16](https://gist.github.com/setuc/3c84e8fc6a2b43a25a563c4fe9e80c75) [17](https://gist.github.com/setuc/f90df3e0f2e5e536ea4dcd5f08568ad2) [18](https://gist.github.com/setuc/24a1c4c2d61576a533d94cfb9814b5b8)