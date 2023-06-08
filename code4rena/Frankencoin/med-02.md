# [M-02] Flaw in logic

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Frankencoin.sol#L172

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Frankencoin.sol#L262

## Impact

`suggestMinter` is an external function that sets the new minter and this minter can mint and burn unlimited amount of tokens to any address.

## Proof of Concept

```solidity
main/contracts/Frankencoin.sol

172: function mint(address _target, uint256 _amount) override external minterOnly {

262: function burn(address _owner, uint256 _amount) override external minterOnly {
```

## Tools Used

Manual audit

## Recommended Migration Steps

Add a restriction to prevent this sequence of actions.
