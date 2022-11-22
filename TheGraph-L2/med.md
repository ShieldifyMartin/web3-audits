# The Graph L2

## Report

### M-01 Return values of `transfer()`/`transferFrom()` not checked

## Impact

Not all IERC20 implementations revert() when there’s a failure in transfer() / transferFrom(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

## Proof of Concept

```solidty
File: /gateway/L1GraphTokenGateway.sol

276: token.transferFrom(escrow, _to, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

## Recommended Mitigation Steps

Perform the check.

### M-02 `safeTransferFrom()` should be used rather than `transferFrom()` wherever possible

## Impact

The transferFrom() method is used instead of safeTransferFrom(), presumably to save gas. I however argue that this isn’t recommended because:

- OpenZeppelin’s documentation discourages the use of transferFrom(), use safeTransferFrom() whenever possible\_
- Given that any NFT can be used for the call option, that have logic in the onERC721Received() function, which is only triggered in the safeTransferFrom() function and not in transferFrom()\_

## Proof of Concept

```solidty
File: /gateway/L1GraphTokenGateway.sol

276: token.transferFrom(escrow, _to, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

## Recommended Mitigation Steps

Call the `safeTransferFrom()` method instead of `transferFrom()` for NFT transfers or the function should be named just like transferERC721Unsafe.
