# Report Part 1
*By Henk Bierlee (4148053) and Kilian Grashoff (4171373)*
## Table of Contents

[TOC]

## 3.1 End-to-End Testing
### Exercise 1
The overall coverage varies between 76.9% and 77.4% (`LauncherSmokeTest` is not consistent between multiple runs).

Examples of classes that are not well-tested are `CollisionInteractionMap` and `DefaultPlayerInteractionMap`. These classes are data structures, which are only used by other code, not directly by the user. Therefore, the smoke test does not cover these classes.
### Exercise 2
See package `nl.tudelft.jpacman.endToEnd`.
### Exercise 3
Scenario 2.3 is the hardest to test, since a ghost needs to be next to the player, which can only be achieved by sleeping the thread, hard coding ghost paths or editing the board. We chose the first option.

Scenario 2.5 is also difficult, since programming a test to complete the game should be very difficult without altering the board. However, there is a way to cheat, since there is a publicly accessible method to remove `Unit`s from `Square`s, so all the pellets except one next to the `Player` can be removed, after which the game can be won in one move.
### Exercise 4
Story 3 is hard to test because many methods relating to `Ghost` are private, since ghosts are moved by the game in normal gameplay, not by the player. Therefore many of the functionality is contained in private methods. We tested scenario 3.1 as an extra exercise.

## 3.2 Boundary Testing
### Exercise 5
For the boundary test, a `Board` instance with a 10x10 grid was constructed, populated with only `BasicSquare` objects. Because of this, the boundaries in the actual game (`getWidth()` and `getHeight()`) have been replaced with the number 10.

#### Boundary conditions for `x >= 0 && x < 10 && y >= 0 && y < 10`
| Variable | Condition | Type | t1   | t2    | t3    | t4   | t5   | t6    | t7    | t8   |
|----------|-----------|------|------|-------|-------|------|------|-------|-------|------|
| x        | \>= 0     | on   | 0    |       |       |      |      |       |       |      |
|          |           | off  |      | -1    |       |      |      |       |       |      |
|          | < 10      | on   |      |       | 10    |      |      |       |       |      |
|          |           | off  |      |       |       | 9    |      |       |       |      |
|          | typical   | in   |      |       |       |      | 8    | 1     | 5     | 6    |
| y        | \>= 0     | on   |      |       |       |      | 0    |       |       |      |
|          |           | off  |      |       |       |      |      | -1    |       |      |
|          | < 10      | on   |      |       |       |      |      |       | 10    |      |
|          |           | off  |      |       |       |      |      |       |       | 9    |
|          | typical   | in   | 2    | 6     | 4     | 5    |      |       |       |      |
| Result   |           |      | TRUE | FALSE | FALSE | TRUE | TRUE | FALSE | FALSE | TRUE |
### Exercise 6
See `nl.tudelft.jpacman.board.ParameterizedBoardTest`. Checkstyle warnings are explained below.

## 3.3 Submit Part 1
### Code submission readiness
Our tests test all user stories exactly as described, contains proper commenting and javadoc, and contains no Checkstyle or Findbugs violations except for the ones explained in the next paragraph.
### Checkstyle and Findbugs violations
Checkstyle reports the following violations:
| Severity 	| Category 	| Rule        	| Message                 	| Line 	|
|----------	|----------	|-------------	|-------------------------	|------	|
|  Warning 	| coding   	| MagicNumber 	| '6' is a magic number.  	| 69   	|
|  Warning 	| coding   	| MagicNumber 	| '10' is a magic number. 	| 70   	|
|  Warning 	| coding   	| MagicNumber 	| '4' is a magic number.  	| 70   	|
|  Warning 	| coding   	| MagicNumber 	| '9' is a magic number.  	| 70   	|
|  Warning 	| coding   	| MagicNumber 	| '5' is a magic number.  	| 70   	|
|  Warning 	| coding   	| MagicNumber 	| '8' is a magic number.  	| 70   	|
|  Warning 	| coding   	| MagicNumber 	| '5' is a magic number.  	| 71   	|
|  Warning 	| coding   	| MagicNumber 	| '10' is a magic number. 	| 71   	|
|  Warning 	| coding   	| MagicNumber 	| '6' is a magic number.  	| 71   	|
|  Warning 	| coding   	| MagicNumber 	| '9' is a magic number.  	| 71   	|
These violations are all in the `ParameterizedBoardTest.data()` method and cannot be avoided in this test, since the magic numbers are our test parameters.
### Log of test results
	Running nl.tudelft.jpacman.endToEnd.MovePlayerTest
	Tests run: 5, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 5.32 sec
	Running nl.tudelft.jpacman.endToEnd.StartGameTest
	Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.039 sec
	Running nl.tudelft.jpacman.endToEnd.SuspendGameTest
	Tests run: 2, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.147 sec
	Running nl.tudelft.jpacman.endToEnd.MoveGhostTest
	Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.038 sec
	Running nl.tudelft.jpacman.board.BoardTest
	Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0 sec
	Running nl.tudelft.jpacman.board.ParameterizedBoardTest
	Tests run: 8, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.012 sec
	Running nl.tudelft.jpacman.board.DirectionTest
	Tests run: 4, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.003 sec
	
	Results :
	
	Tests run: 22, Failures: 0, Errors: 0, Skipped: 0
### Additional adequacy in `jpacman-framework`
Since we have not altered `jpacman-framework` all additional adequacy stems from extra guarantees about the code provided by our tests.
* `StartGameTest` guarantees that the game starts correctly.
* `MovePlayerTest` guarantees that the player is able to move, and that moving results in gameplay events (such as consuming pellets or dying).
* `MoveGhostTest` guarantees that ghosts are able to move to empty cells. The rest of these scenarios has not been implemented, since they are optional.
* `SuspendGameTest` guarantees that the game can be suspended correctly, and restarted afterwards.
* `ParameterizedBoardTest` is a unit test, which guarantees that the `Board.withinBorders()` method functions as intended, according to the one-by-one domain testing strategy.