# Wrong Oracle Data Validation

## Summary

**Likelihood:**
Low, because it require chainlink oracle to return wrong data

**Impact:**
High, because accounting would be with wrong data and even the protocol can be drained

## Vulnerability Detail

Chainlink’s `latestRoundData` might return stale or wrong data even though `updateAt` property was checked for staleness. Despite the fact you added such issue as "acceptable risk" it has a serious impact for the whole protocol and should be mitigated.

## Impact

In `chainlinkAdaptor` you are using Chainlink’s latestRoundData API, but there are missing validation checks. This could lead to returning wrong data according to the Chainlink documentation and even to entirely drained protocols.

## Code snippet

```solidity
46: (, rawPrice, , updatedAt, ) = IChainlink(chainlink).latestRoundData();

47: (, int256 USDCPrice,, uint256 USDCUpdatedAt,) = IChainlink(USDCSource).latestRoundData();
```

https://github.com/sherlock-audit/2023-04-jojo-martin-petrov03/blob/main/smart-contract-EVM/contracts/adaptor/chainlinkAdaptor.sol#L46

https://github.com/sherlock-audit/2023-04-jojo-martin-petrov03/blob/main/smart-contract-EVM/contracts/adaptor/chainlinkAdaptor.sol#L47

```solidity
28: (, int256 price,, uint256 updatedAt,) = IChainLinkAggregator(chainlink).latestRoundData();

29: (, int256 USDCPrice,, uint256 USDCUpdatedAt,) = IChainLinkAggregator(USDCSource).latestRoundData();
```

https://github.com/sherlock-audit/2023-04-jojo-martin-petrov03/blob/main/JUSDV1/src/oracle/JOJOOracleAdaptor.sol#L28

https://github.com/sherlock-audit/2023-04-jojo-martin-petrov03/blob/main/JUSDV1/src/oracle/JOJOOracleAdaptor.sol#L29

## Tool used

Manual Review

## Recommendation

Consider adding the missing checks:

```solidity
require(roundId >= answeredInRound, “Wrong oracle data”);
require(answer > 0, "Chainlink price <= 0");
require(answeredInRound >= roundId, "Stale price");
```
