# Report Part 3
*By Henk Bierlee (4148053) and Kilian Grashoff (4171373)*

## 5.1 State machines
###Exercise 16
State machine model for the state that is implicit in the requirements
contained in doc/scenarios.md:
![State machine diagram](http://i.imgur.com/rBMs0D2.png)

### Exercise 17
Transition tree derived from the state machine:
![Transition tree](http://i.imgur.com/oCqjbDo.png)
Note: we only made the tree for the Player part of our state machine, since the Ghost class can not be end-to-end tested properly (see our report for Part 1, exercise 4).

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
|     Game suspended     | Player at valid square |    no action   |        no action       |        no action       |        no action       |    no action    |       no action       |
| Player at valid square |        no action       | Game suspended | Player at valid square | Player at valid square | Player at valid square |    Game over    |        Game won       |
|        Game over       |      not specified     |    no action   |        no action       |        no action       |        no action       |    no action    |       no action       |
|        Game won        |      not specified     |    no action   |        no action       |        no action       |        no action       |    no action    |       no action       |

####Exercise 19
All valid paths from the root to leaves of our transition tree are already tested by existing tests. Specifically (Test Case ID's can be found in exercise 17):

Case 0 is tested by `SuspendGameTest.testSuspendGame()`
Case 1 is tested by `MovePlayerTest.testMoveToEmptySquare()`
Case 2 is tested by `MovePlayerTest.testMoveToWall()`
Case 3 is tested by `MovePlayerTest.testMoveToSquareWithPellet()`
Case 4 is tested by `MovePlayerTest.testPlayerDies()`
Case 5 is tested by `MovePlayerTest.testPlayerWins()`

However, to show that we can apply the State Machine testing method, we have rewritten these tests in terms of states in the `nl.tudelft.jpacman.group8.GameStateRoundTripTest` class. These tests use our `SingleLevelLauncher` to load the `small_map.txt` map.

The test cases for (state, event) pairs not contained in our diagram remain. These are the cases that have `no action` or `not specified` as result in the table. Not specified does not need to be tested (we do not care what the result is, as it is not defined in the requirements, so there is nothing to test). So we need to derive test cases for all `no action` entries in the table. All entries corresponding to a `move` event and one state (in the same row) can be compressed into one test case, since the move condition does not matter if there is no resulting action. This results in the following test cases, which can be found in the `GameStateInvalidEventTest`:

| Test Case ID |       Start state      |    Events   |   Result  |
|:------------:|:----------------------:|:-----------:|:---------:|
|       6      |     Game suspended     |  press stop | no action |
|       7      |     Game suspended     |     move    | no action |
|       8      | Player at valid square | press start | no action |
|       9      |        Game over       |  press stop | no action |
|       10      |        Game over       |     move    | no action |
|       11      |        Game won        |  press stop | no action |
|       12      |        Game won        |     move    | no action |

####Exercise 20
User story S2.5 was adjusted (but ended up being implemented seperately for single and multi levels. See exercise 22):

	Scenario S2.5: Player advances level, extends S2.1
	When    I have eaten the last pellet;
	 and    I am not in the last level;
	Then    I advance to the next level.

User story S2.6 was added:

	Scenario S2.6: Player wins, extends S2.1
	When    I have eaten the last pellet;
	 and    I am in the last level;
	Then    I win the game.
The updated `scenarios.md` file can be found in the doc folder of our project.

####Exercise 21
![Extended state machine diagram](http://i.imgur.com/axvJ6Ml.png)

####Exercise 22
The updated state machine yielded two extra test cases:

| Test Case ID |       Start state      |    Events   |   Result  |
|:------------:|:----------------------:|:-----------:|:---------:|
|       13      |     Game suspended     |  move [to last Pellet], next level | Player at valid square |
|       14      |     Game suspended     |  move [to last Pellet], win [level = lastLevel] | Game won |

The 'Eat last pellet' scenario (Test case ID: 5) appeared to be a likely candidate for test reuse. However, our implementation supports both single level and multi level games, and both are fully tested. The end result of that scenario is different for single level and multi level games, so we had to add tests instead of merely adjusting the existing single level testing.

####Exercise 23, 24
We decided on the following class hierarchy for this exercise:

We extended `Launcher` to `CustomLevelLauncher`, so we could load our own levels. Then, we made two subclasses of `CustomLevelLauncher`: the `SingleLevelLauncher` and the `MultiLevelLauncher`. This way we can support both types of games. On the `Game` side, we made the subclass `MultiLevelGame`, which mirrors `SinglePlayerGame` (from the framework), only with multiple level support.

(We initially considered to just extend `SinglePlayerGame` - which might have made things simpler - but decided against it. Firstly, the assignment told us to extend `Game`, not `SinglePlayerGame`. Secondly, extending `SinglePlayerGame` would result in the inheritance of obsolete fields in `MultiLevelGame`, such as `level`.)

####Exercise 25, 26
Our test suite accommodates the class structure of our extensions. 

#####Structure
- `AbstractRoundTripTest` (in `nl.tudelft.jpacman.group8.gamestate.roundtrip`) contains tests cases which test behavior of any `Game` subclass (Test Case ID's: 0-4). 
- `SingleLevelRoundTripTest` contains test cases which test behavior specific to `SinglePlayerGame` (Test Case ID: 5).
- `MultiLevelRoundTripTest` contains test cases which test behavior specific to `MultiLevelGame` (Test Case ID's: 13-14).  

#####Implementation
`AbstractRoundTripTest` forces subclasses to use their own type of `Launcher` and `Game`. Such a subclass has the responsibility to set the `SubclassLauncher` and `SubclassGame` fields in the `setUp` method and return their values in the `getGame` and `getLauncher` methods. 	This way, the type that is to be tested is dynamically selected for each test.

####Exercise 27
The implementation of `MultiLevelGame` was straightforward. Override `getLevel` to return the current level according to a `levelIndex`. Override `gameWon` to increment the `levelIndex`, or call `stop` when there are no more levels.
