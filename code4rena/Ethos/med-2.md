# [M-02] Deprecated `safeApprove()`

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Vault/contracts/abstract/ReaperBaseStrategyv4.sol#L74

## Impact

The function `safeApprove` function has been deprecated by OpenZeppelin team.

## Proof of Concept

```solidity
74: IERC20Upgradeable(want).safeApprove(vault, type(uint256).max);
```

## Tools Used

Manual review

## Recommended Mitigation Steps

Use `safeIncreaseAllowance` and `safeDecreaseAllowance` instead.
