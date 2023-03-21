# Canto

## Gas Optimizations Report

### G-01 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **1** instances of this issue:_

```solidity
File: /src/Turnstile.sol

127: function withdraw(uint256 _tokenId, address payable _recipient, uint256 _amount)
```

https://github.com/code-423n4/2022-11-canto/blob/main/CIP-001/src/Turnstile.sol

### G-02 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **1** instances of this issue:_

```solidity
File: /src/Turnstile.sol

balances[_tokenId] += msg.value;
```

https://github.com/code-423n4/2022-11-canto/blob/main/CIP-001/src/Turnstile.sol
