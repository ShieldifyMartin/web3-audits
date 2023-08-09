# [M-02] Not pausable inbound functionality

https://github.com/sherlock-audit/2023-02-carapace-martin-petrov03/blob/main/contracts/core/pool/ProtectionPool.sol#L1029

## Impact

All state-changing methods should be pausable, especially all inbound ones. This is a serious security issue due to the fact that it could still be deposits of underlying token to the pool and the corresponding sToken shares minting to the receiver whild the contract is paused. Therefore, medium severity fits well.

## Proof of Concept

```solidity
1029: function _deposit(uint256 _underlyingAmount, address _receiver) internal {
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Add `whenNotPaused()` modifier to the `_deposit` function.
