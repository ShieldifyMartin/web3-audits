# [M-02] Front-runnable trade function

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/SpigotedLineLib.sol#L134

## Proof of Concept

Let’s say Alice had approval of 100. Now the treasury custodian reduced the approval to 50. Alice could frontrun the setApprovalFor of 50, and withdraw 100 as it was before. Then withdraw 50 with the newly set approval. So the alice could withdraw 150.

```solidity

134: IERC20(sellToken).approve(swapTarget, amount);
```

## Recommended Migration Steps

Instead of setting the given amount, check whether the previous approval is spend.
