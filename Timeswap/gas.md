# Timeswap

## Gas Optimizations Report

### Duplicated `require()`/`revert()` checks should be refactored to a modifier or function

_There are **8** instances of this issue:_

```solidity
if (isQuote) revert Quote();
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol

```solidity
if (uint256(transaction) >= 2) revert InvalidTransaction();
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/enums/Transaction.sol
