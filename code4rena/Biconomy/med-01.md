# [M-01] Two-Factor Ownership Transfer should be used

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/SmartAccount.sol#L109

## Impact

It’s possible to lose the ownership under specific circumstances.

## Proof Of Concept

Because of human error it’s possible to set a new invalid owner. When you want to change the owner’s address it’s better to propose a new owner, and then accept this ownership with the new wallet.

```solidity
function setOwner(address _newOwner) external mixedAuth {
    require(_newOwner != address(0), "Smart Account:: new Signatory address cannot be zero");
    address oldOwner = owner;
    owner = _newOwner;
    emit EOAChanged(address(this), oldOwner, _newOwner);
}
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Implement Two-Factor Authentication with pending and approve ownership functions.
