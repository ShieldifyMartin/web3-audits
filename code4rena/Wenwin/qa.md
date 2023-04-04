## Introduction

Wenwin QA report was done by martin, with a main focus on the low severity and non-critical security aspects of the implementaion and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

When minting NFTs, the code uses `_safeMint` function of the OZ reference implementation. This function is safe because it checks whether the receiver can receive ERC721 tokens. The can prevent the case that a NFT will be minted to a contract that cannot handle ERC721 tokens. However, this external function call creates a security loophole. Specifically, the attacker can perform a reentrant call inside the onERC721Received callback. More detailed information why a reeentrancy can occur - https://blocksecteam.medium.com/when-safemint-becomes-unsafe-lessons-from-the-hypebears-security-incident-2965209bda2a.

```solidity
74: _mint(msg.sender, amount);
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/staking/Staking.sol

```solidity
19:  _mint(msg.sender, INITIAL_SUPPLY);

26: _mint(account, amount);
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/LotteryToken.sol

### L-02 `revert()` should be used instead of `assert()`

The comment above clarifies that it must hold after this call, in such case it's better to add the check and revert providing a description of necessary.

```solidity
147: assert(initialPot > 0);
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/LotterySetup.sol

### L-03 `ClaimedRewards` event is called early

Event is actually emit before the action, event emits should be moved after the action. Otherwise it might cause incorrectly emitted event.

```solidity
155: emit ClaimedRewards(beneficiary, claimedAmount, rewardType);
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol

### L-04 Misleading function

Should be a modifier instead.

```solidity
279: function requireFinishedDraw(uint128 drawId) internal view override {
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/Lottery.sol

### N-01 Use a more recent version of solidity

Using a floating pragma statement is discouraged as code can compile to different bytecodes with different compiler versions. Use a stable pragma statement to get a deterministic bytecode. Also use the latest Solidity version in all files if possible to get all compiler features, bugfixes, and optimizations.

### N-02 Not using the named `return` variables anywhere in the `function` is confusing

Consider changing the variable to be an unnamed one

```solidity
120: function fixedReward(uint8 winTier) public view override returns (uint256 amount) {
```

https://github.com/code-423n4/2023-03-wenwin/blob/main/src/LotterySetup.sol

### N-03 Missing NatSpec

Add NatSpec docs for all external functions so their intentions are clear.
