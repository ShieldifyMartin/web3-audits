# [M-02] Non-Compliant Allowance Logic

## Summary

Using deprecated function that is also fron-runnable

## Vulnerability Detail

This behavior is not natively present when calling approve() and can therefore easily create unintended reverts that lock funds in smart contracts that use this code. The front-running vector is also present.

## Impact

These functions can be front-run.

## Code Snippet

```solidity
63: _telcoin.safeApprove(address(_referral), _telcoin.balanceOf(address(this)));

72: IERC20(token).safeApprove(_aggregator, 0);

73: IERC20(token).safeApprove(_aggregator, amount);

80: _telcoin.safeApprove(address(_referral), _telcoin.balanceOf(address(this)));
```

https://github.com/sherlock-audit/2023-02-telcoin-martin-petrov03/blob/main/telcoin-audit/contracts/staking/FeeBuyback.sol#L63
https://github.com/sherlock-audit/2023-02-telcoin-martin-petrov03/blob/main/telcoin-audit/contracts/staking/FeeBuyback.sol#L72
https://github.com/sherlock-audit/2023-02-telcoin-martin-petrov03/blob/main/telcoin-audit/contracts/staking/FeeBuyback.sol#L73
https://github.com/sherlock-audit/2023-02-telcoin-martin-petrov03/blob/main/telcoin-audit/contracts/staking/FeeBuyback.sol#L80

## Tool used

Manual Review

## Recommendation

The function `safeApprove` function has been deprecated by OpenZeppelin team. Avoid using it and use `safeIncreaseAllowance` and `safeDecreaseAllowance` instead since USDT is not one of the supported tokens.
