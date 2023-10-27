## Introduction

Venus QA report was done by martin and anonresercher, with a main focus on the low severity and non-critical security aspects of the implementation and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

## Findings Summary

| ID      | Title                                                         | Severity     |
| ------- | ------------------------------------------------------------- | ------------ |
| [NC-01] | Prefer battle-tested code over reimplementing common patterns | Non-Critical |
| [NC-02] | Redundant check                                               | Non-Critical |
| [NC-03] | Event should be emitted for important state changes           | Non-Critical |
| [NC-04] | Event was emitted earlier                                     | Non-Critical |
| [NC-05] | Bad formatting                                                | Non-Critical |
| [NC-05] | Typos                                                         | Non-Critical |

### [NC-01] Prefer battle-tested code over reimplementing common patterns

Replace the `nonReentrant` modifier in VToken with the nonReentrant from OpenZeppelin, since it is well-tested and optimized.

### [NC-02] Redudant check

The `setPoolRegistry` function can be called only by the owner, so it is unlikely to be called with address(0), but even if it happens it can be called again with correct data.

```solidity
54: require(_poolRegistry != address(0), "ProtocolShareReserve: Pool registry address invalid");

111: require(shortfallContractAddress_ != address(0), "Risk Fund: Shortfall contract address invalid");

127: require(pancakeSwapRouter_ != address(0), "Risk Fund: PancakeSwap address invalid");
```

https://github.com/code-423n4/2023-05-venus/blob/main/contracts/RiskFund/RiskFund.sol

### [NC-03] Event should be emitted for important state changes

```solidity
578: function healAccount(address user) external {
```

https://github.com/code-423n4/2023-05-venus/blob/main/contracts/Comptroller.sol

### [NC-04] Event was emitted earlier

The `AuctionRestarted` event is actually emitted before the `_startAuction`, event emits should be moved after the creation. Otherwise, it might be incorrectly emitted.

```solidity
283: emit AuctionRestarted(comptroller, auction.startBlock);
```

https://github.com/code-423n4/2023-05-venus/blob/main/contracts/Shortfall/Shortfall.sol

### [NC-05] Bad formatting

```solidity
211: * @param comptroller  Comptroller address(pool).

```

https://github.com/code-423n4/2023-05-venus/blob/main/contracts/RiskFund/RiskFund.sol

### [NC-06] Typos

```diff
-- * @return proxyAddress The the Comptroller proxy address
++ * @return proxyAddress The Comptroller proxy address
```

https://github.com/code-423n4/2023-05-venus/blob/main/contracts/Pool/PoolRegistry.sol
