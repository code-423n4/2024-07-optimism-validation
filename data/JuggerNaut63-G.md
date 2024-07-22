## Description:
GAS OPTIMIZATION IN INCREMENTS
++i costs less gas compared to i++ or i += 1 for unsigned integers.
In i++, the compiler has to create a temporary variable to store the initial value. This is not the case with ++i in which the value is directly incremented and returned, thus, making it a cheaper alternative.
Code snippet:
 - https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L607
 - https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L94
 - https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/cannon/PreimageOracle.sol#L698

GAS OPTIMIZATION FOR STATE VARIABLES
Plus equals (+=) costs more gas than addition operator. The same thing happens with minus equals (-=). Therefore, x +=y costs more gas than x = x + y.
Code snippet:
 - https://github.com/code-423n4/2024-07-optimism/blob/70556044e5e080930f686c4e5acde420104bb2c4/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L854

## Remediation
- Consider changing the post-increments (i++) to pre-increments (++i) as long as the value is not used in any calculations or inside returns. Make sure that the logic of the code is not changed.
- addition operator over plus equals
- subtraction operator over minus equals
- division operator over divide equals
- multiplication operator over multiply equals

