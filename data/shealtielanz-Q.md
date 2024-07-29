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

