# Tigris Trade

## Gas Optimizations Report

### G-01 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **4** instances of this issue:_

https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/PairsContract.sol
https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/Trading.sol
https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/GovNFT.sol
https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/BondNFT.sol

### G-02 Splitting `require()` statements that use && saves gas

Instead of using `&&` on single require check using two require checks can save gas

_There are **1** instances of this issue:_

```solidity
File: /utils/TradingLibrary.sol

117: _priceData.price < assetChainlinkPrice+assetChainlinkPrice*2/100 &&
```

https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/utils/TradingLibrary.sol

### G-03 Duplicated `require()`/`revert()` checks should be refactored to a modifier or function

_There are **1** instances of this issue:_

```solidity
File: /PairsContract.sol

require(_name.length > 0, "!Asset");
```

https://github.com/code-423n4/2022-12-tigris/blob/main/contracts/PairsContract.sol
