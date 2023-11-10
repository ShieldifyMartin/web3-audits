# [H-01] Potentially drain more funds than allowed

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L52
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol#L72
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L18
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositTradeHelper.sol#L19
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol#L94

## Impact

Let’s say UserOne had approval of 100. Now the treasury custodian reduced the approval to 50. UserOne could frontrun the approve of 50, and withdraw 100 as it was before. Then withdraw 50 with the newly approval. So the UserOne could withdraw 150.

## Proof of Concept

```solidity
52: baseToken.approve(address(depositHook), _fee);

72: baseToken.approve(address(withdrawHook), _fee);
```

```solidity
18: collateral.getBaseToken().approve(address(collateral), type(uint256).max);

19: collateral.approve(address(swapRouter), type(uint256).max);
```

```solidity
94: collateral.approve(address(_redeemHook), _expectedFee);
```

## Tools Used

Manual audit

## Recommended Migration Steps

Instead of setting the given amount, one can reduce from the current approval. By doing so, it checks whether the previous approval is spend.
