# [M-01] Loss of precision

## Summary

Loss of precision

## Vulnerability Detail

`uint256` value is truncated to a `uint32` value, the upper 224 bits will be discarded, resulting in a loss of precision.

## Impact

It's important to carefully evaluate the potential impact of any loss of precision when truncating the upper bits of block.timestamp to a uint32 and to ensure that the code is designed to handle this correctly.

## Code Snippet

```solidity
102: context.baseStrategy.vaultState.lastSettlementTimestamp = uint32(block.timestamp);
```

https://github.com/sherlock-audit/2023-02-notional-martin-petrov03/blob/main/leveraged-vaults/contracts/vaults/Curve2TokenConvexVault.sol#L102

## Tool used

Manual Review

## Recommendation

Avoid casting
