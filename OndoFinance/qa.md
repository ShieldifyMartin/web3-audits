# Ondo Finance

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **2** instances of this issue:_

```solidity
260: cash.mint(user, cashOwed);

791: cash.mint(refundee, cashAmountBurned);
```

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/cash/CashManager.sol

### L-02 `require()` should be used instead of `assert()`

_There are **1** instances of this issue:_

```solidity
97: assert(cashProxyAdmin.owner() == guardian);
```

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/cash/factory/CashFactory.sol

### L-03 Very old version of solidity

This could have serious security implications due to the bug fixes in the new versions.

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/JumpRateModelV2.sol

### L-04 Modifiers shouldn’t update state

_There are **2** instances of this issue:_

```solidity
1434:  modifier nonReentrant() {
```

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cCash/CTokenCash.sol

```solidity
1437: modifier nonReentrant() {
```

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cToken/CTokenModified.sol

### N-01 Non-floating `pragma` should be used

Not using same version of solidity for different smart contracts is inconcistency, but it could also lead to security holes.

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cCash/CCashDelegate.sol

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cToken/CTokenDelegate.sol

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/JumpRateModelV2.sol

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cCash/CCash.sol

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cToken/CErc20.sol

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cCash/CTokenInterfacesModifiedCash.sol

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cToken/CTokenInterfacesModified.sol

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cErc20ModifiedDelegator.sol

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cCash/CTokenCash.sol

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cToken/CTokenModified.sol
