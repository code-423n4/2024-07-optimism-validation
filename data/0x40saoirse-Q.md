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



# The `claimCredit` function is exposed to a reentrancy attack

https://github.com/code-423n4/2024-07-optimism/blob/main/packages/contracts-bedrock/src/dispute/FaultDisputeGame.sol#L747-L761


# Description: 
The claimCredit function is prone to a reentrancy attack. Malicious code could reenter the function. However since `credit[_recipient] = 0;` will have already been set zero the function will revert and no damage will be done.

On re-entering the function the function will simply revert
```solidity
 `if (recipientCredit == 0) revert NoCreditToClaim();`
```

However it is still better to following best coding practices and use `https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/ReentrancyGuard.sol` This prevent any reentrancy altogether.


# Recommended Mitigation:
Use `ReentrancyGuard` and the `nonReentrant` modifier
```solidity
    function claimCredit(address _recipient) external nonReentrant;
```