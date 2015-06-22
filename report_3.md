# Report Part 3
*By Henk Bierlee (4148053) and Kilian Grashoff (4171373)*

## 5.1 State machines
### Exercise 17

| Test Case ID |    Start state    |                Events               |         End State        |
|:------------:|:-----------------:|:-----------------------------------:|:------------------------:|
|       0      | Game suspended 0  |       press start, press stop       |     Game suspended 1     |
|       1      | Game suspended 0  | press start, move [to empty Square] | Player at valid Square 1 |
|       2      | Game suspended 0  |     press start, move [to Wall]     | Player at valid Square 2 |
|       3      | Game suspended 0  |    press start, move [to Pellet]    | Player at valid Square 3 |
|       4      | Game suspended 0  |     press start, move [to Ghost]    |         Game over        |
|       5      | Game suspended 0  |  press start, move [to last Pellet] |         Game won         |


### Exercise 18

|         STATES         |         events         |                |                        |                        |                        |                 |                       |
|:----------------------:|:----------------------:|:--------------:|:----------------------:|:----------------------:|:----------------------:|:---------------:|:---------------------:|
|                        |       press start      |   press stop   | move [to empty Square] |     move [to Wall]     |    move [to Pellet]    | move [to Ghost] | move [to last Pellet] |
|     Game suspended     | Player at valid square |                |                        |                        |                        |                 |                       |
| Player at valid square |                        | Game suspended | Player at valid square | Player at valid square | Player at valid square |    Game over    |        Game won       |
|        Game over       |                        |                |                        |                        |                        |                 |                       |
|        Game won        |                        |                |                        |                        |                        |                 |                       |

####Exercise 18