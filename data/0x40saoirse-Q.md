Multiple Imports of `Types.sol` file into `FaultDisputeGame.sol`

https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L14

https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L17

# Description:
There are multiple imports of the Types object from Types.sol

```solidity
    import { Types } from "src/libraries/Types.sol";
```

# Impact:
This is unnecessary code bloat and can be cleaned up by using only one import


# Recommended Mitigation:
Ensure there is only one import of the Types object.