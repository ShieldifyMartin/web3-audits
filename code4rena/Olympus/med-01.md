# [M-01] Front-runnable `approve` function

## Summary

`_deposit` and `_withdraw` functions are front-runnable

## Vulnerability Detail

USDT approval race condition - to do an approve call the allowance should be either 0 or type(uint256).max.

## Impact

The methods `_deposit` and `_withdraw` from WstethLiquidityVault can be monitored for transactions and front-ran. Imagine the following scenario:

1. Alice had approval of 100
2. Treasury custodian reduced the approval to 50
3. Alice could frontrun the setApprovalFor of 50, and withdraw 100 as it was before. Then withdraw 50 with the newly set approval. So the alice could withdraw 150.
   frozen

## Code Snippet

```solidity
131: ohm.approve(address(vault), ohmAmount_);

132: pairToken.approve(address(vault), pairAmount_);

139: pool.approve(address(auraPool.booster), lpAmountOut);

178: pool.approve(address(vault), lpAmount_);
```

https://github.com/sherlock-audit/2023-02-olympus-martin-petrov03/blob/main/src/policies/lending/WstethLiquidityVault.sol

## Tool used

Manual Review

## Recommendation

Use increase/decrease allowance instead.
