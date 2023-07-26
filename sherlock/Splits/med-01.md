# [M-01] Single-step ownership transfer can be dangerous

## Summary

**Likelihood**
Low since it requires a mistake from the previous owner's side

**Impact**
High because the contract ownership can be lost forever

## Vulnerability Detail

Because of human error it’s possible to set a new invalid owner. When you want to change the owner’s address it’s better to propose a new owner, and then accept this ownership with the new wallet.

## Impact

It’s possible to lose the ownership under specific circumstances.

## Code Snippet

```solidity
45: function transferOwnership(address owner_) public virtual onlyOwner {
46:    $owner = owner_;
47:    emit OwnershipTransferred(msg.sender, owner_);
48:}
```

https://github.com/sherlock-audit/2023-04-splits-martin-petrov03/blob/main/splits-utils/src/OwnableImpl.sol#L45

## Tool used

Manual Review

## Recommendation

Consider using an role-based access control approach instead of a single admin role as well as a timelock for important admin actions. In the custom transfer ownership implementation add pending and approve owner functions.
