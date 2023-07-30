# [M-02] Chainlink’s oracle might return stale results

## Summary

Missing important oracle response validation checks.

## Vulnerability Detail

Across these contracts, you are using Chainlink’s latestRoundData API, but there is a complete lack of oracle response validation.

## Impact

Not checking how stale the answer is could lead to stale prices according to the Chainlink documentation and even to entirely drained protocols.

## Code Snippet

https://github.com/sherlock-audit/2023-05-USSD-martin-petrov03/blob/main/ussd-contracts/contracts/oracles/StableOracleDAI.sol#L48

https://github.com/sherlock-audit/2023-05-USSD-martin-petrov03/blob/main/ussd-contracts/contracts/oracles/StableOracleWBTC.sol#L23

## Tool used

Manual Review

## Recommendation

Consider using the line which imports the returned properties and adds the corresponding checks:

```diff
-- (, int256 price, , , ) = priceFeedDAIETH.latestRoundData();

++ (uint80 roundID, int256 price, uint256 startedAt, uint256 timeStamp, uint80 answeredInRound) = priceFeedDAIETH.latestRoundData();
++ require(roundID >= answeredInRound, “Wrong oracle data”);
++ require(price > 0, "Chainlink price <= 0");
++ require(answeredInRound >= roundID, "Stale price");
++ require(block.timestamp - startedAt <= MAX_ORACLE_FRESHNESS_SECONDS>, "Last price update was too long ago.");
```
