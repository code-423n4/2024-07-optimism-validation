# NC 1: Order not followed as defined in struct
When assigning value to `startingOutputRoot` as `OutputRoot`, order of struct items is not followed as it was declared.

`OutputRoot` defined as 

```solidity
struct OutputRoot {
    Hash root;
    uint256 l2BlockNumber;
}
```
at line 19 of `packages/contracts-bedrock/src/dispute/lib/Types.sol`

while when assigning value, it is used as:
```solidity
        startingOutputRoot = OutputRoot({ l2BlockNumber: rootBlockNumber, root: root });
```

# NC 2: Checkers Effects pattern is not followed
Check that should have been done at the start of function is done at the of the function.

```solidity
        if (parent.counteredBy != address(0)) revert DuplicateStep();
```

at line 307 of `packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol`