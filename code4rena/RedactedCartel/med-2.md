# OpenZeppelin's `Pausable` Centralisation Vulnerability

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexGmx.sol#L685

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexGmx.sol#L712

## Impact

It's a centralisation vulnerability if an owner can pause not only inbound, but outbound functionality. It's a best practice to be able to pause inbound (stake) functionality but not outbound (withdraw) methods

# Proof of Concept

```solidity
function redeemPxGlpETH(
        uint256 amount,
        uint256 minOut,
        address receiver
    )
        external
        whenNotPaused
        nonReentrant
        returns (
            uint256,
            uint256,
            uint256
        )
    {
        return _redeemPxGlp(address(0), amount, minOut, receiver);
    }
```

```solidity
function redeemPxGlp(
        address token,
        uint256 amount,
        uint256 minOut,
        address receiver
    )
        external
        whenNotPaused
        nonReentrant
        returns (
            uint256,
            uint256,
            uint256
        )
    {
        if (token == address(0)) revert ZeroAddress();
        if (!gmxVault.whitelistedTokens(token)) revert InvalidToken(token);

        return _redeemPxGlp(token, amount, minOut, receiver);
    }
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Remove the `whenNotPaused` modifier from outbound (withdraw) functions
