# [M-02] Single-step ownership transfer can be dangerous

## Summary

**Likelihood:**
Low, because it requires mistake from the previous owner

**Impact:**
High, because ownership will be lost forever

## Vulnerability Detail

Because of human error it’s possible to set a new invalid owner. When you want to change the owner’s address it’s better to propose a new owner, and then accept this ownership with the new wallet.

## Impact

It’s possible to lose ownership under specific circumstances.

## Code Snippet

https://github.com/sherlock-audit/2023-04-jojo-martin-petrov03/blob/main/smart-contract-EVM/contracts/impl/Perpetual.sol#L14

## Tool used

Manual Review

## Recommendation

Consider using an `Ownable2Step` instead of `Ownable` as well as a timelock for important admin actions.
