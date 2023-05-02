# [M-01] Single-step ownership transfer can be dangerous

## Impact

It’s possible to lose the ownership under specific circumstances.

## Code snippet

Because of human error it’s possible to set a new invalid owner. When you want to change the owner’s address it’s better to propose a new owner, and then accept this ownership with the new wallet.

```solidity
58: function setAdminContract(address _admin) external onlyOwner {
59:    require(_admin != address(0));
60:    adminContract = _admin;
61: }
```

https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/GRVT/CommunityIssuance.sol#L58

## Tools Used

Manual Review

## Recommended Mitigation Steps

Consider using an role-based access control approach instead of a single admin role as well as a timelock for important admin actions.
