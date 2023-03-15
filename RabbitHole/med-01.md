# [M-01] Reentrancy attack vector

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/RabbitHoleReceipt.sol#L103

## Impact

The recipient contract may define any arbitrary logic to be executed from `mint` function. However the recommended check-effect-interaction coding pattern is followed, still reentrancy is possible so medium severity matches.

## Proof of Concept

```solidity
103: _safeMint(to_, newTokenID);
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

One of the following options should be implemented:

1. Import OpenZeppelin ReentrancyGuard library and add `nonReentrant` modifier to the `mint` function.

2. Add custom function execution locking functianality.
