## Introduction

Caviar Private Pools QA report was done by martin, with a main focus on the low severity and non-critical security aspects of the implementaion and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

## Findings Summary

| ID      | Title                                                 | Severity     |
| ------- | ----------------------------------------------------- | ------------ |
| [L-01]  | Missing `nonReentrant` modifier                       | Low          |
| [L-02]  | User is unable to withdraw mis-sent funds             | Low          |
| [L-03]  | No upper limit validation on user-supplied parameters | Low          |
| [NC-01] | Use a safe pragma statement                           | Non-Critical |
| [NC-02] | Incomplete NatSpec                                    | Non-Critical |
| [NC-03] | Follow best practices when concatenating strings      | Non-Critical |
| [NC-04] | Typos                                                 | Non-Critical |

### [L-01] Missing `nonReentrant` modifier

`_safeMint` performs external function call that creates a security loophole. Specifically, the attacker can perform a reentrant call inside the onERC721Received callback. More detailed information why a reeentrancy can occur - https://blocksecteam.medium.com/when-safemint-becomes-unsafe-lessons-from-the-hypebears-security-incident-2965209bda2a.

https://github.com/code-423n4/2023-04-caviar/blob/main/src/Factory.sol#L71

### [L-02] User is unable to withdraw mis-sent funds

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert. Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds. There is `withdraw` function which allows only the admin to withdraw the earned protocol fees (Ether or Tokens)

https://github.com/code-423n4/2023-04-caviar/blob/main/src/Factory.sol#L55

https://github.com/code-423n4/2023-04-caviar/blob/main/src/PrivatePool.sol#L134

### [L-03] No upper limit validation on user-supplied parameters

`deadline` parameter is not validated properly. Check it to not be more than 100 days for example.

https://github.com/code-423n4/2023-04-caviar/blob/main/src/EthRouter.sol#L99

### [NC-01] Use a safe pragma statement

Using a floating pragma ^0.8.19 statement is discouraged as code can compile to different bytecodes with different compiler versions. Use a stable pragma statement to get a deterministic bytecode.

```solidity
File: /src/Factory.sol

File: /src/PrivatePoolMetadata.sol

File: /src/EthRouter.sol

File: /src/PrivatePool.sol

File: /src/interfaces/IStolenNftOracle.sol
```

### [NC-02] Incomplete NatSpec

Following the Solidity Style Guide sections, try to write as detailed comments as possible to make it easier for auditors, developers or other technical guys to understand the purpose of each particular feature and its parameters.

Description of the `_payRoyalties` param is missing

https://github.com/code-423n4/2023-04-caviar/blob/main/src/Factory.sol#L71

### [NC-03] Follow best practices when concatenating strings

It is recommended to use `string.concat` instead of `abi.encodePacked` for better readability and maintainability of your code.

https://github.com/code-423n4/2023-04-caviar/blob/main/src/PrivatePoolMetadata.sol

### [N-04] Typos

```solidity
251: // add the royalty fee amount to the net input aount
```

https://github.com/code-423n4/2023-04-caviar/blob/main/src/PrivatePool.sol
