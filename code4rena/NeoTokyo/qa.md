## Introduction

Neo Tokyo QA report was done by martin, with a main focus on the low severity and non-critical security aspects of the implementaion and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

# [L-01] Use `_safeMint` instead of `_mint` to prevent NFTs from being locked in a contract that does not handle such tokens

When minting NFTs, the code uses `_safeMint` function of the OZ reference implementation. This function is safe because it checks whether the receiver can receive ERC721 tokens. The can prevent the case that a NFT will be minted to a contract that cannot handle ERC721 tokens. However, this external function call creates a security loophole. Specifically, the attacker can perform a reentrant call inside the onERC721Received callback. More detailed information why a reeentrancy can occur - https://blocksecteam.medium.com/when-safemint-becomes-unsafe-lessons-from-the-hypebears-security-incident-2965209bda2a.

```solidity
102: _mint(msg.sender, _amount);

124: _mint(_to, reward);

127: _mint(TREASURY, daoCommision);

154: _mint(TREASURY, treasuryShare);
```

https://github.com/code-423n4/2023-03-neotokyo/blob/main/contracts/staking/BYTES2.sol

# [L-02] Useless function

## Impact

The permission needed to pause this function is the same with the permissiong needed to call `configureLP`, which on the other hand is the only function paused with `lockLP`.

## Proof of Concept

```solidity
1721: function lockLP () external hasValidPermit(UNIVERSAL, CONFIGURE_LP) {
```

https://github.com/code-423n4/2023-03-neotokyo/blob/main/contracts/staking/NeoTokyoStaker.sol

## Tools used

Manual review

## Recommended

Remove this function and move the check in `lockLP`.

# [L-03] Use selectors to the safe functions instead

Using safe functions will remove the need to check the call success.

```solidity
190: /// The `transferFrom` selector for ERC-20 and ERC-721 tokens.
191: bytes4 constant private \_TRANSFER_FROM_SELECTOR = 0x23b872dd;

192: /// The `transfer` selector for ERC-20 tokens.
193: bytes4 constant private _TRANSFER_SELECTOR = 0xa9059cbb;
```

https://github.com/code-423n4/2023-03-neotokyo/blob/main/contracts/staking/NeoTokyoStaker.sol

# [NC-01] Use non-floating version of solidity

Using a floating pragma ^0.8.19 statement is discouraged as code can compile to different bytecodes with different compiler versions. Use a stable pragma statement to get a deterministic bytecode.

https://github.com/code-423n4/2023-03-neotokyo/blob/main/contracts/staking/BYTES2.sol
https://github.com/code-423n4/2023-03-neotokyo/blob/main/contracts/staking/NeoTokyoStaker.sol

# [NC-02] Code is not properly formatted

Run a formatter on the code, for example use the `prettier-solidity` plugin.

https://github.com/code-423n4/2023-03-neotokyo/blob/main/contracts/staking/BYTES2.sol
https://github.com/code-423n4/2023-03-neotokyo/blob/main/contracts/staking/NeoTokyoStaker.sol

# [NC-03] Not necessary assembly

Yul should not be used where it is not mandatory because it increases code complexity and opens possible memory conflicts.

```solidity
824: function _stringEquals (
```

https://github.com/code-423n4/2023-03-neotokyo/blob/main/contracts/staking/NeoTokyoStaker.sol

# [NC-04] Not emitted event

`Claim` event in only declared and never actually emitted. Consider removing it.

```solidity
553: event Claim (
```

https://github.com/code-423n4/2023-03-neotokyo/blob/main/contracts/staking/NeoTokyoStaker.sol
