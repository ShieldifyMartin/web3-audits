# [M-01] Parameter change could lead to wrong calculations and unnecessary actions

## Summary

**Likelihood:**
Medium, because it requires `rewardsEarned_ > ajnaBalance`

**Impact:**
Medium, because accounting might be wrong, which may lead to loss of funds for the ajnaToken or for the user, and unnecessary actions to perform `_updateBucketExchangeRates` and `_updateBucketExchangeRateAndCalculateRewards` would be executed anyways

## Impact

Such implementation is in contrast with programming best practices and it'll force unnecessary additional actions to execute `_updateBucketExchangeRates` and `_updateBucketExchangeRateAndCalculateRewards` and can even lead to loss of funds in edge case scenarios.

## Proof of Concept

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-core/src/RewardsManager.sol#L815

## Tool Used

Manual Review

## Recommended Mitigation Steps

The first choice is to redesign the logic to store the currently up-to-date users' rewards in the storage variable and perform calculations on it. Then if `rewardsEarned_ > ajnaBalance` check is true, you can subtract the user rewards with ajnaBalance in storage and execute the transfer.
The second choice is to simply revert the transaction in case `rewardsEarned_ > ajnaBalance`, to prevent unexpected behavior.
