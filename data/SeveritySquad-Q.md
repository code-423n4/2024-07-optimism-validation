`Low Risk & Non Critical Findings`
# Report
## Low Risk Findings 
**Low Risk Findings List**
| Number | Issue Details | Instances |
|-----:|----|-----|
| L-01 | Incorrect checks in preImage Oracle allow for bypass validations against OOB errors | 2 |
| L-02 |  Theoretical Overflow at LibPosition#wrap() could potentially lead to manipulation of position | 1 |
| L-03 | Hashing leafs of 64 bytes allows the preimage to be vulnerable to second node preimage attacks | 2 |
| L-04 | Alignment Check for lw //MIPS | 1 |
| L-05 | Opcode differences btw the 2 chains are not handled whenever executing the single instruction trace | 4 |
| L-06 |  lhu (load halfword unsigned): Address must be divisible by 2 | 1 |
| L-07 | Unsafe cast of the bond can lead to loss of funds even when the bond > max.128 by 1 | 1 |
| L-08 | The used Proxy for the DisputeGameFactory.sol is not fully EIP1967 compliant  | 1 |
| L-09 | partial parts can be forced to be stored in the preimageoracle even when not finalized | 2 |
| L-10 | Lack of input validation at loadPrecompilePreimagePart() | 1 |

# L-01 Incorrect checks in preImage Oracle allow for bypass validations against OOB errors
## Summary 
## Vulnerability Details


# L-02 Theoretical Overflow at LibPosition#wrap() could potentially lead to manipulation of position 

The wrap() function could overflow whenever `indexAtDepth` is set to `type(uint128).max`. 

```solidity
    function wrap(uint8 _depth, uint128 _indexAtDepth) internal pure returns (Position position_) {
        assembly {
            // gindex = 2^{_depth} + _indexAtDepth
// @audit-issue Overflow possible when `_indexAtDepth` is set at `type(uint128).max`.
            position_ := add(shl(_depth, 1), _indexAtDepth)
        }
    }   
```
## Recommended Mitigation Steps

Please add a check before the assembly code to make sure that `_indexAtDepth` would never overflow and return a wrong `_position`.


# L-08 The used Proxy for the DisputeGameFactory.sol is not fully EIP1967 compliant

**NOTE:**
*- This should be seen as in scope, this issue pinpoint a functionality of [DisputeGameFactoryProxy.sol](https://docs.optimism.io/chain/addresses) the proxy that is used by the DisputeGameFactory.sol (in-scope)*

The [setImplementation function](https://etherscan.io/address/0xe5965Ab5962eDc7477C8520243A95517CD252fA9#code#F1#L101) is designed to set a new implementation contract address for upgrading the DisputeGameFactory.sol contract to a new logic. This function stores the new implementation address in the EIP1967 implementation slot and immediately applies the new logic. The current function does not validate whether the new address is actually a contract. This validation step is crucial and is included in the official ERC1967 implementation examples. 

### Impact:

Failing to validate that the new implementation address is a contract can lead to serious issues. Specifically, if a non-contract address is set as the implementation, the proxy would be unable to delegate calls correctly, leading to potentially severe operational failures. This could render the proxy dysfunctional and may result in a significant disruption of services, especially if the function is executed in a live production environment.

### Recommendation:

To align with the official ERC1967 standard and enhance the robustness of the setImplementation function, it is recommended to include a check to validate that the new implementation address is indeed a contract. This can be done by adding a line to require that the new implementation address satisfies the Address.isContract condition, as follows:

```solidity
require(Address.isContract(newImplementation), "ERC1967: new implementation is not a contract");	
```

References:	
- ERC1967: Link to the [official documentation and examples](https://eips.ethereum.org/EIPS/eip-1967#abstract:~:text=/**%0A%20%20%20%20%20*%20%40dev%20Stores%20a,newImplementation%3B%0A%20%20%20%20%7D)
- Solidity Documentation on Address.isContract: Link to [Solidity docs](https://docs.soliditylang.org/en/v0.8.6/units-and-global-variables.html#address-related)
- [Identic issue from Euler contest on Cantina](https://cantina.xyz/code/41306bb9-2bb8-4da6-95c3-66b85e11639f/findings/320) *(got confirmed by judge as low)*

