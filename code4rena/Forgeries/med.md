# [M-01] Return values of `transferFrom()` not checked

https://github.com/code-423n4/2022-12-forgeries/blob/main/src/VRFNFTRandomDraw.sol#L187
https://github.com/code-423n4/2022-12-forgeries/blob/main/src/VRFNFTRandomDraw.sol#L295
https://github.com/code-423n4/2022-12-forgeries/blob/main/src/VRFNFTRandomDraw.sol#L315

## Impact

1. Not all IERC20 implementations revert() when there’s a failure in `transferFrom()`. The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment.
2. Since the IERC20 interface requires a boolean return value, attempting to transfer ERC20s with missing return values will revert. This means Forgeries payment terminals cannot support a number of popular ERC20s, including USDT and BNB.

## Proof of Concept

```solidity
IERC721EnumerableUpgradeable(settings.token).transferFrom(
    msg.sender,
    address(this),
    settings.tokenId
)

IERC721EnumerableUpgradeable(settings.token).transferFrom(
    address(this),
    msg.sender,
    settings.tokenId
);

IERC721EnumerableUpgradeable(settings.token).transferFrom(
    address(this),
    owner(),
    settings.tokenId
);
```

## Tools Used

Manual audit

## Recommended Migration Steps

Perform the checks.
