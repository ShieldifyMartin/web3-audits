# [M-04] Chainlink’s `latestRoundData` might return stale results

### Impact

Chainlink’s latestRoundData API is used, but there are serious missing checks. Not checking how stale the answer is could lead to stale prices according to the Chainlink documentation and even to entirely drained protocols (LUNA).

## Code snippet

https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/PriceFeed.sol#L245
https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/PriceFeed.sol#L272

## Tools Used

Manual Review

### Recommended Migration Steps

Consider adding the missing checks for stale data:

```diff
++ require(roundId >= answeredInRound, “Wrong oracle data”);
++ require(answer > 0, "Chainlink price <= 0");
++ require(answeredInRound >= roundID, "Stale price");
++ require(block.timestamp - updatedAt <= MAX_ORACLE_FRESHNESS_SECONDS>, "Last price update was too long ago.");
```
