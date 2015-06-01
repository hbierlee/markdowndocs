# Report Part 2
*By Henk Bierlee (4148053) and Kilian Grashoff (4171373)*

## 3.1 Mocks
### Exercise 9
See the `MapParserTest` class
### Exercise 10
See the `MapParserTest` class
##3.2 Testing Collisions
### Exercise 11
We opted for a non-binary decision table. There are 3 types of units (player, ghost and pellet), so there are 9 possible collisions. However, since pellets cannot be colliders (as they don't move), those columns have been omitted. However, we thought it was useful to test the mirrored version of the player-ghost collision.

The table should be read as follows: the first column lists the colliders. The next columns represent a rule (or situation) each, in which a collision takes place with the unit in that column - the collidee. The bottom cell of the column is the expected outcome of the collision. For example, column 3 specifies rule 2, in which a player collides with a ghost, which should result in the player dying. This is the exact same logic as used in the non-binary decision table in lecture 5, slide 32.
| collider/rule       | rule 1     | rule 2 | rule 3 | rule 4      | rule 5     | rule 6     |
|--------|------------|--------|--------|-------------|------------|------------|
| **player** | player     | ghost  | pellet | -           | -          | -          |
| **ghost**  | -          | -      | -      | player      | ghost      | pellet     |
| **outcome**   | *no actions* | *player dies*   | *player eats*   | *player dies* | *no actions* | *no actions* |
### Exercise 12
See the `Add test case for PlayerCollisions class` commit. (note: around this time the decision table was incomplete, so some test cases are missing)
### Exercise 13
See final submission. The '*no actions*' outcome has been implemented with the 	`Mockito.verifyZeroInteractions` method.
### Exercise 14
The coverage at the end of the previous assignment was about 77 percent. Now it is 85.6 percent, so we have achieved an increase of about 8.6 percent. The new tests in 3.2 have covered most of `CollisionInteractionMap` (96.8%) and all of `DefaultPlayerInteractionMap` and `PlayerCollisions` (100%).

The tests cover 6 of the 9 collisions that are possible at this point. Extending JPacman with more units types would require more tests. Also, if `Pellet` objects would gain the ability to move, then collisions with a pellet as collider have not been covered. With the parallel class hierarchy, all collision tests are run for both `DefaultPlayerInteractionMap` and `PlayerCollisions`. Future `CollisionInteractionMap` implementations can be tested by these tests with minimal effort. When such future collision maps are designed with unique functionality not found in other implementations, those tests can also be implemented in the test case for that implementation (and they won't be run for the other implementations).