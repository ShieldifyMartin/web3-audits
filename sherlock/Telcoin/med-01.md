# [M-01] Protocol is will not work on most of the supported blockchains due to hardcoded MATIC contract address.

## Summary

Strongly dependent logic

## Vulnerability Detail

Hardcoded MATIC address

## Impact

The protocol is will not work on most of the supported blockchains due to the hardcoded MATIC contract address.

## Code Snippet

```solidity
address constant public MATIC = 0x0000000000000000000000000000000000001010;
```

https://github.com/sherlock-audit/2023-02-telcoin-martin-petrov03/blob/main/telcoin-audit/contracts/staking/FeeBuyback.sol#L18

## Tool used

Manual Review

## Recommendation

Set `MATIC` value from a constructor parameter.
