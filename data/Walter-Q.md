## Validation Check Error in Move Position Depth in FaultDisputeGame#step
The step function in the ``FaultDisputeGame`` contract contains a vulnerability in the validation of the move position. The current code checks if the move position is not equal to ``MAX_GAME_DEPTH + 1``, which is incorrect. According to the documentation(comments above the check), the validation should ensure that the move position depth is exactly one less than ``MAX_GAME_DEPTH``, not one more.
```
// INVARIANT: A step cannot be made unless the move position is 1 below the `MAX_GAME_DEPTH` @audit -- LOW
if (stepPos.depth() != MAX_GAME_DEPTH + 1) revert InvalidParent();
```
### Mitigation
The vulnerability arises because ``MAX_GAME_DEPTH`` is incorrectly incremented by 1 in the check. The code should instead verify that the depth is ``MAX_GAME_DEPTH - 1`` to align with the correct game logic and expected behavior.
```
// INVARIANT: A step cannot be made unless the move position is 1 below the `MAX_GAME_DEPTH`
if (stepPos.depth() != MAX_GAME_DEPTH - 1) revert InvalidParent();
```