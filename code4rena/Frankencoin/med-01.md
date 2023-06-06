# [M-01] Paying `_applicationFee` can be omitted

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Frankencoin.sol#L83

## Impact

When `totalSupply` is initially 0 and also 0 is passed for `_applicationFee` the transaction won't revert. In this way, the `_applicationFee` won't be paid.

## Proof of Concept

```solidity
main/contracts/Frankencoin.sol

84: if (_applicationPeriod < MIN_APPLICATION_PERIOD && totalSupply() > 0) revert PeriodTooShort();

85: if (_applicationFee < MIN_FEE  && totalSupply() > 0) revert FeeTooLow();
```

## Tools Used

Manual audit

## Recommended Migration Steps

Remove the `totalSupply() > 0` check or use `||` instead of `&&`.
