# [M-02] Deprecated `safeApprove()`

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L358

## Impact

This behavior is not natively present when calling `approve()` and can therefore easily create unintended reverts that lock funds in smart contracts that use this code. The front-running vector is also present.

## Proof of Concept

```solidity
358: IERC20(_token).safeApprove(_bathToken, _amount);
```

## Tools Used

Manual review

## Recommendation

The function `safeApprove` function has been deprecated by OpenZeppelin team. Avoid using it and use `safeIncreaseAllowance` and `safeDecreaseAllowance` for cToken approvals.
