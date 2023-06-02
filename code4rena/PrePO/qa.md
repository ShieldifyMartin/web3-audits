# PrePo

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **3** instances of this issue:_

```solidity
File: /apps/smart-contracts/core/contracts/Collateral.sol

58: _mint(_recipient, _collateralMintAmount);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol

```solidity
File: /apps/smart-contracts/core/contracts/PrePOMarket.sol

70: longToken.mint(msg.sender, _amount);

71: shortToken.mint(msg.sender, _amount);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol

### N-01 Use a more recent version of solidity

### N-02 `require()`/`revert()` statements should have descriptive reason strings

_There are **1** instances of this issue:_

```solidity
File: /apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

30: require(_newMinReservePercentage <= PERCENT_DENOMINATOR, ">100%");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

### N-03 Needles `return`

_There are **1** instances of this issue:_

```solidity
File: /apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

48: return (_newLongToken, _newShortToken);
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol
