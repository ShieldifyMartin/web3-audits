## Introduction

Ethos QA report was done by martin, with a main focus on the low severity and non-critical security aspects of the implementaion and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

### L-01 Consider implementing Two-Factor `updateCollateralRatios` with pending and approve functions

Because of admin human error it’s possible to set a new `_MCR` and `_CCR` invalid values. When you want to change these values, it’s better to propose a change, and then approve this changes.

_There are **1** instances of this issue:_

```solidity
85: function updateCollateralRatios(
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/CollateralConfig.sol

### L-02 `revert()` should be used instead of `assert()`

The comment above clarifies that it is impossible to open a trove with zero withdrawn LUSD, in such case it's better to add the check and revert providing a description of necessary.

_There are **2** instances of this issue:_

```solidity
128: assert(MIN_NET_DEBT > 0);

197: assert(vars.compositeDebt > 0);
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/BorrowerOperations.sol

### L-03 `_safeMint()` should be used rather than `_mint()` wherever possible

When minting NFTs, the code uses `_safeMint` function of the OZ reference implementation. This function is safe because it checks whether the receiver can receive ERC721 tokens. The can prevent the case that a NFT will be minted to a contract that cannot handle ERC721 tokens. However, this external function call creates a security loophole. Specifically, the attacker can perform a reentrant call inside the onERC721Received callback. More detailed information why a reeentrancy can occur - https://blocksecteam.medium.com/when-safemint-becomes-unsafe-lessons-from-the-hypebears-security-incident-2965209bda2a.

_There are **2** instances of this issue:_

```solidity
427: _lusdToken.mint(lqtyStakingAddress, LUSDFee);

513: _lusdToken.mint(_account, _LUSDAmount);
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/BorrowerOperations.sol

### L-04 `ActivePoolCollateralBalanceUpdated` and `CollateralSent` events are called early

Event is actually emit before the action, event emits should be moved after the action. Otherwise it might cause incorrectly emitted event.

_There are **6** instances of this issue:_

```solidity
176: emit ActivePoolCollateralBalanceUpdated(_collateral, collAmount[_collateral]);

177: emit CollateralSent(_collateral, _account, _amount);
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/ActivePool.sol

```solidity
354: emit CollateralGainWithdrawn(msg.sender, collateral, amount);

400: emit CollateralGainWithdrawn(msg.sender, collateral, amount);

788: emit CollateralSent(_collateral, msg.sender, _amount);
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/StabilityPool.sol

```solidity
243: emit CollateralSent(msg.sender, collateral, amounts[i]);
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LQTY/LQTYStaking.sol

### N-01 Use non-floating pragma version

Using a floating pragma ^0.8.0 statement is discouraged as code can compile to different bytecodes with different compiler versions. Use a stable pragma statement to get a deterministic bytecode. Also use latest Solidity version to get all compiler features, bugfixes and optimizations

_There are **4** instances of this issue:_

```solidity
File: /Ethos-Vault/contracts/ReaperVaultV2.sol

File: /Ethos-Vault/contracts/ReaperVaultERC4626.sol

File: /Ethos-Vault/contracts/abstract/ReaperBaseStrategyv4.sol

File: /Ethos-Vault/contracts/ReaperStrategyGranarySupplyOnly.sol
```

### N-02 Use a more recent version of solidity

_There are **8** instances of this issue:_

```solidity
File: /Ethos-Core/contracts/CollateralConfig.sol

File: /Ethos-Core/contracts/BorrowerOperations.sol

File: /Ethos-Core/contracts/TroveManager.sol

File: /Ethos-Core/contracts/ActivePool.sol

File: /Ethos-Core/contracts/StabilityPool.sol

File: /Ethos-Core/contracts/LQTY/CommunityIssuance.sol

File: /Ethos-Core/contracts/LQTY/LQTYStaking.sol

File: /Ethos-Core/contracts/LUSDToken.sol
```

### N-03 Misleading functions

The so called 'Require' wrapper functions are actually modifiers. Modifiers were introduced in Solidity version 0.4.0 and it is a best practice to use them.

_There are **4** instances of this issue:_

```solidity
522: // --- 'Require' wrapper functions ---
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/BorrowerOperations.sol

```solidity
1520: // --- 'require' wrapper functions ---
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/TroveManager.sol

```solidity
130: // --- 'require' functions ---
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LQTY/CommunityIssuance.sol#L127

```solidity
249: // --- 'require' functions ---
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LQTY/LQTYStaking.sol

### N-04 Remove commented code

_There are **1** instances of this issue:_

```solidity
File: /Ethos-Core/contracts/TroveManager.sol
```

### N-05 Formatting issues

_There are **2** instances of this issue:_

```solidity
849: require( msg.sender == address(activePool), "StabilityPool: Caller is not ActivePool");
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/StabilityPool.sol

```solidity
25: mapping( address => uint) public stakes;
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LQTY/LQTYStaking.sol

### N-06 Hardcoded values

_There are **1** instances of this issue:_

```solidity
58: distributionPeriod = 14 days;
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LQTY/CommunityIssuance.sol

### N-07 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

_There are **1** instances of this issue:_

```solidity
40: uint256 public constant DEGRADATION_COEFFICIENT = 10**18;
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Vault/contracts/ReaperVaultV2.sol

### N-08 Not using the named `return` variables anywhere in the `function` is confusing

Consider changing the variable to be an unnamed one.

_There are **2** instances of this issue:_

```solidity
29: function asset() external view override returns (address assetTokenAddress) {
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Vault/contracts/ReaperVaultERC4626.sol

```solidity
104: function _liquidateAllPositions() internal override returns (uint256 amountFreed) {
```

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Vault/contracts/ReaperStrategyGranarySupplyOnly.sol
