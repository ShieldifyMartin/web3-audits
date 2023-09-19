## Introduction

Canto Identity Subprotocol QA report was done by martin, with a main focus on the low severity and non-critical security aspects of the implementaion and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

# [L-01] Library Confusion

## Summary

Confusion caused by insufficient research

## Description

Solmate's LibString.sol supports UTF-8 characters, including arbitrary characters that can be represented in UTF-8 encoding. It includes a wide range of functions for manipulating strings as well which work with any valid UTF-8 encoded string (arbitrary Unicode characters including).

## Code snippet

```solidity
41: /// @dev Generates an on-chain SVG with a new line after 40 bytes. Line splitting generally supports UTF-8 multibyte characters and emojis, but is not tested for arbitrary UTF-8 characters
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-bio-protocol/src/Bio.sol#L41

# [L-02] Use `_safeMint` instead of `_mint` to prevent NFTs from being locked in a contract that does not handle such tokens

When minting NFTs, the code uses `_safeMint` function of the OZ reference implementation. This function is safe because it checks whether the receiver can receive ERC721 tokens. This can prevent the case that a NFT will be minted to a contract that cannot handle ERC721 tokens. However, this external function call creates a security loophole. Specifically, the attacker can perform a reentrant call inside the onERC721Received callback. More detailed information why reentrancy can occur - https://blocksecteam.medium.com/when-safemint-becomes-unsafe-lessons-from-the-hypebears-security-incident-2965209bda2a.

Additionally in `Tray.sol` there is a valid reason to not use `_safeMint` if there is such for `Bio.sol`, `ProfilePicture.sol`, and `Namespace.sol` provide it or consider using `_safeMint`.

```solidity
126: _mint(msg.sender, tokenId);
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-bio-protocol/src/Bio.sol#L126

```solidity
86: _mint(msg.sender, tokenId);
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-pfp-protocol/src/ProfilePicture.sol#L86

```solidity
177: _mint(msg.sender, namespaceIDToMint);
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Namespace.sol#L177

# [L-03] Events inconsistency for important state change

Although \_mint already emits an event, in the owe additionally emits one because of the name

```solidity
150: function buy(uint256 _amount) external {
```

https://github.com/code-423n4/2023-03-canto-identity/blob/main/canto-namespace-protocol/src/Tray.sol#L150
