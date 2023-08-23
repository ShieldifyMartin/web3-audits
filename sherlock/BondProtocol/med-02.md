# [M-02] Insufficient oracle data validation

## Summary

Missing oracle data feed validation check

## Vulnerability Detail

Missing oracle data feed validation check

## Impact

Each time the oracle updates the price feed, it generates a new roundId. The roundId should match the timestamp of the price data, which can also be obtained from oracle's smart contract. The roundIds should be in sequential order with no missing values.

## Code Snippet

```solidity
135: (uint80 roundId, int256 priceInt, , uint256 updatedAt, uint80 answeredInRound) = feed_
```

https://github.com/sherlock-audit/2023-02-bond-martin-petrov03/blob/main/bonds/src/BondChainlinkOracle.sol#L135

## Tool used

Manual Review

## Recommendation

Add the following check and keep reverting if true.

```diff
- answeredInRound != roundId

+ (roundId >= answeredInRound);
```
