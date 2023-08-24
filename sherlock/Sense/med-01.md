# [M-01] Centralisation Vulnerability

## Summary

Centralisation vulnerability due to wrong pausing functionality

## Vulnerability Detail

Outbound functionality can be paused

## Impact

It's a centralisation vulnerability if an owner can pause not only inbound but outbound functionality. It's a best practice to be able to pause inbound functionality but not outbound methods.

## Code Snippet

```solidity
305: function redeem(

338: function collect(
```

https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/Divider.sol#L305
https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/Divider.sol#L338

## Tool used

Manual Review

## Recommendation

Remove `whenNotPaused` modifier from `collect` and `redeem` functions to make them usable when contract state is paused.
