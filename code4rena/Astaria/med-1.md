# [M-01] Deprecated `safeApprove`

https://github.com/code-423n4/2023-01-astaria/blob/main/src/ClearingHouse.sol#L148

## Impact

The reason this code is so bug-prone is because this behavior is not natively present when calling `approve()` and can therefore easily create unintended reverts that lock funds in smart contracts that use this code. Based on the documentation in the readmes and at the top of the file, one would assume that it is doing a safer, more-compatible version of the approve call (when it is in fact doing a more-restrictive version of the function-call)

## Proof of Concept

```solidity
148: ERC20(paymentToken).safeApprove(
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Use `safeIncreaseAllowance` and `safeDecreaseAllowance` instead.
