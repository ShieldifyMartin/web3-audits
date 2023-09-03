# [M-01] No way of using pausability in Auction, OperatorRewardsCollector, SocializingPool and StaderOracle

## Impact

The `pause()` and `unpause()` methods are actually internal, so if your contract does not expose them externally you have no way of using pausability. In contrast in ETHx, PermissionedNodeRegistry, StaderStakePoolsManager and UserWithdrawalManager for example the pattern implementation is totally correct.

## Proof of Concept

There aren't externally exposed `pause` and `unpause` methods, so it's impossible to use this functionality as expected.

## Tools Used

Manual Review

## Recommended Migration Steps

The `pause()` and `unpause()` methods should be exposed in the code of the contract which inhertits PausableUpgradeable. Add these external methods with the proper access control in `Auction.sol`, `OperatorRewardsCollector.sol`, `SocializingPool.sol` and `StaderOracle.sol`.
