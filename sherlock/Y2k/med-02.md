# [M-02] Single-step ownership transfer can be dangerous

## Summary

Protocol control can be lost forever

**Likelihood:**
Low, because it requires the old admin to make a mistake in new admin address

**Impact:**
High, because the control of the protocol will be lost forever

## Vulnerability Detail

Because of human error it’s possible to set a new invalid owner. When you want to change the owner’s address it’s better to propose a new owner, and then accept this ownership with the new wallet.

## Impact

It’s possible to lose the ownership under specific circumstances.

## Code Snippet

```solidity
171: function changeOwner(address _newOwner) external onlyOwner {
```

https://github.com/sherlock-audit/2023-03-Y2K-martin-petrov03/blob/main/Earthquake/src/v2/TimeLock.sol#L171

## Tool used

Manual Review

## Recommendation

Implement Two-Factor Authentication with pending and approve ownership functions.
