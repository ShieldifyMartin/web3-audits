# M-01 Return values of `transfer()`/`transferFrom()` not checked

## Impact

`TokenTransferrer#_transferFrom` calls IERC20 transfer and transferFrom directly. There are two issues with using this interface directly:

1. `TokenTransferrer#_transferFrom` function does not check the return value of these calls. Tokens that return `false` rather than revert to indicate failed transfers may silently fail rather than reverting as expected.
2. Since the IERC20 interface requires a boolean return value, attempting to transfer ERC20s with missing return values will revert. This means `Looks Rare` payment terminals cannot support a number of popular ERC20s, including USDT and BNB.

## Proof of Concept

```solidity
File: /contracts/TokenTransferrer.sol

22: IERC721(collection).transferFrom(address(this), recipient, tokenId);
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/TokenTransferrer.sol

## Recommended Migration Steps

`safeTransferFrom()` should be used rather than `transferFrom()`
