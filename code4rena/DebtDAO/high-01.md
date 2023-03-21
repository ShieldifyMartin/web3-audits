# [H-01] Oracle Data Feed can be outdated yet used anyways which will impact payment logic

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/oracle/Oracle.sol#L22

## Impact

The current implementation of FeedRegistryInterface is used without roundId, startedAt, timeStamp and answeredInRound properties. Not checking how stale the answer is could lead to stale prices according to the Chainlink documentation.

## Proof of Concept

```solidity
function getLatestAnswer(address token) external returns (int) {
    (
        /* uint80 roundID */,
        int price,
        /* uint80 startedAt */,
        /* uint80 timeStamp */,
        /* uint80 answeredInRound */
    ) = registry.latestRoundData(token, Denominations.USD);

    return price;
}
```

## Tools Used

VScode

## Recommended Migration Steps

Consider adding the missing checks for stale data.

```solidity
require(answer > 0, "Chainlink price <= 0");
require(answeredInRound >= roundID, "Stale price");
require(block.timestamp -  updatedAt <= MAX_ORACLE_FRESHNESS_SECONDS>, "Last price update was too long ago.");
```
