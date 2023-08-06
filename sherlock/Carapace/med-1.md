# [M-01] Reentrancy

https://github.com/sherlock-audit/2023-02-carapace-martin-petrov03/blob/main/contracts/core/pool/ProtectionPool.sol#L1029

## Impact

The recipient contract may define any arbitrary logic to be executed from `_deposit` function. However the recommended check-effect-interaction coding pattern is followed, still reentrancy is possible so medium severity matches.

## Proof of Concept

```solidity
1037: _safeMint(_receiver, _sTokenShares);
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

One of the following options should be implemented:

1. As OpenZeppelin ReentrancyGuard library is imported, add `nonReentrant` modifier to the `_deposit` function.

2. Add custom function execution locking functianality.
