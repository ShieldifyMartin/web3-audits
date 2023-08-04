# [M-01] Chainlink’s `latestRoundData` might return stale results

## Summary

Chainlink’s `latestRoundData` might return stale results

## Vulnerability Detail

Chainlink’s `latestRoundData` might return stale results

## Impact

Across these contracts, you are using Chainlink’s `latestRoundData` API, but there is no check on `updatedAt` property. Not checking how stale the answer is could lead to stale prices according to the Chainlink documentation and even to entirely drained protocols.

## Code Snippet

```solidity
299: (uint80 roundID, int256 price, , , uint80 answeredInRound) = priceFeed
```

https://github.com/sherlock-audit/2023-03-Y2K-martin-petrov03/blob/main/Earthquake/src/v2/Controllers/ControllerPeggedAssetV2.sol#L299

## Tool used

Manual Review

## Recommendation

Consider adding the missing checks for stale data:

```solidity
(uint80 roundID, int256 price, , uint256 updatedAt, uint80 answeredInRound) = priceFeed.latestRoundData();
require(block.timestamp -  updatedAt <= MAX_ORACLE_FRESHNESS_SECONDS>, "Last price update was too long ago.");
```
