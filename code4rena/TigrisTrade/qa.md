# Tigris Trade

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **4** instances of this issue:_

```solidity
File: /Trading.sol

204: position.mint(

333: position.mint(
```

https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/Trading.sol

```solidity
File: /GovNFT.sol

56: super._mint(to, tokenId);
```

https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/GovNFT.sol

```solidity
File: /StableToken.sol

32: _mint(account, amount);
```

https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/StableToken.sol

### N-01 `revert()` statements should have descriptive reason strings

```solidity
File: /Trading.sol

File: /TradingExtension.sol

File: /Referrals.sol
```

### N-02 Interfaces should be extracted to separate files

_There are **2** instances of this issue:_

```solidity
File: /Trading.sol

File: /utils/TradingLibrary.sol
```

### N-03 Non-floating `pragma` should be used

_There are **11** instances of this issue:_

```solidity
File: /Trading.sol

File: /TradingExtension.sol

File: /utils/TradingLibrary.sol

File: /Position.sol

File: /PairsContract.sol

File: /Referrals.sol

File: /GovNFT.sol

File: /StableToken.sol

File: /StableVault.sol

File: /Lock.sol

File: /BondNFT.sol
```

### N-04 Use a more recent version of solidity

_There are **11** instances of this issue:_

```solidity
File: /Trading.sol

File: /TradingExtension.sol

File: /utils/TradingLibrary.sol

File: /Position.sol

File: /PairsContract.sol

File: /Referrals.sol

File: /GovNFT.sol

File: /StableToken.sol

File: /StableVault.sol

File: /Lock.sol

File: /BondNFT.sol
```

### N-05 Could be `immutable`

_There are **3** instances of this issue:_

```solidity
File: /TradingExtension.sol

22: IPairsContract private pairsContract;

23: IReferrals private referrals;

24: IPosition private position;
```

https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/TradingExtension.sol
