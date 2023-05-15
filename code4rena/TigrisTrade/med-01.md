# [M-01] Unexpected Ether

https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/Trading.sol#L654
https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/Trading.sol#L675

## Impact

Typically when ether is sent to a contract, it must execute either the fallback function, or another function described in the contract. There are two exceptions to this, where ether can exist in a contract without having executed any code. Contracts which rely on code execution for every ether sent to the contract can be vulnerable to attacks where ether is forcibly sent to a contract.

## Proof of Concept

```solidity
if (tigAsset.balanceOf(address(this)) != _balBefore + _margin) revert BadDeposit();

if (IERC20(_outputToken).balanceOf(address(this)) != _balBefore + _toMint/(10**(18-ExtendedIERC20(_outputToken).decimals()))) revert BadWithdraw();
```

## Tools Used

Manual audit

## Recommended Migration Steps

Don't expect the contract balance to be specific amount and use comparsion and in no way strict equal check.

```solidity
if (tigAsset.balanceOf(address(this)) < _balBefore + _margin) revert BadDeposit();
```

```solidity
if (IERC20(_outputToken).balanceOf(address(this)) < _balBefore + _toMint/(10**(18-ExtendedIERC20(_outputToken).decimals()))) revert BadWithdraw();
```
