# Blur Exchange

## Report

### M-01 Return values of `transfer()`/`transferFrom()` not checked

Not all IERC20 implementations revert() when there’s a failure in transfer()/transferFrom(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

_There are **8** instances of this issue:_

```solidty
File: /ExecutionDelegate.sol

78: IERC721(collection).transferFrom(from, to, tokenId);

93: IERC721(collection).safeTransferFrom(from, to, tokenId);

109: IERC1155(collection).safeTransferFrom(from, to, tokenId, amount, "");

125: return IERC20(token).transferFrom(from, to, amount);
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/ExecutionDelegate.sol

```solidity
File: /BlurExchange.sol

508: payable(to).transfer(amount);

511: executionDelegate.transferERC20(weth, from, to, amount);

538: executionDelegate.transferERC721(collection, from, to, tokenId);

540: executionDelegate.transferERC1155(collection, from, to, tokenId, amount);
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol

### M-02 `safeTransferFrom()` should be used rather than `transferFrom()` wherever possible

In other case it should be named just like `transferERC721Unsafe`.

_There are **1** instances of this issue:_

```solidity
File: /ExecutionDelegate.sol

125: return IERC20(token).transferFrom(from, to, amount);
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/ExecutionDelegate.sol
