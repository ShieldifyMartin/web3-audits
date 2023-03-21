[M-01] Usage of deprecated `latestAnswer` to get current price

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L82
https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol#L116

### Impact

According to Chainlink's documentation, the `latestAnswer` function is deprecated. This function does not error if no answer has been reached but returns 0. Besides, the latestAnswer is reported with 18 decimals for crypto quotes but 8 decimals for FX quotes (See Chainlink FAQ for more details).

## Proof of Concept

```solidity
File: /src/Oracle.sol

82: uint price = feeds[token].feed.latestAnswer();

116: uint price = feeds[token].feed.latestAnswer();
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

## Recommended Migration Steps

Use the `latestRoundData` function to get the price. It is important to add validation on the return data if the price is stale or invalid.

Implementation example:

```solidity
(uint80 roundId, int256 answer, uint256 startedAt, uint256 updatedAt, uint80 answeredInRound) = oracle.latestRoundData();

require(answer > 0, "Chainlink price <= 0");
require(block.timestamp -  updatedAt <= MAX_ORACLE_FRESHNESS_SECONDS>, "Last price update was too long ago.");
```
