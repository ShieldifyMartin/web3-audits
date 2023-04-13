# [M-01] `getGGPPriceInAVAX` might return stale results

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/Staking.sol#L259
https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/Staking.sol#L276
https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/Staking.sol#L296
https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/Staking.sol#L310

## Impact

Across these contracts, you are using `getGGPPriceInAVAX`, but there isn't a check on timestamp. This could lead to stale prices and fund loss if there is no recent update and significant price change. That is why I believe it's Medium severity.

## Proof of Concept

```solidity
(uint256 ggpPriceInAvax, ) = oracle.getGGPPriceInAVAX();
```

## Tools Used

Manual audit

## Recommended Migration Steps

Consider adding the missing checks for stale data.

```solidity
(uint256 ggpPriceInAvax, uint256 timestamp) = oracle.getGGPPriceInAVAX();

if(timestamp < MAX_ORACLE_FRESHNESS_SECONDS) {
    revert STALE_PRICE_DATA();
}
```
