# M-02 `safeTransferFrom()` should be used rather than `transferFrom()`

## Impact

_HolographOperator_ `transferFrom` calls IERC20 `transferFrom` directly. There are two issues with using this interface directly:

1. HolographOperator `_transferFrom` function does not check the return value of these calls. Tokens that return `false` rather than revert to indicate failed transfers may silently fail rather than reverting as expected.
2. Since the IERC20 interface requires a boolean return value, attempting to transfer ERC20s with missing return values will revert. This means Holograph payment terminals cannot support a number of popular ERC20s, including USDT and BNB.

## Proof of Concept

_There are **2** instances of this issue:_

```solidity
File: /contracts/HolographOperator.sol

839: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

889: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

## Recommended Mitigation Steps

Call the `safeTransferFrom()` method instead of `transferFrom()`.
