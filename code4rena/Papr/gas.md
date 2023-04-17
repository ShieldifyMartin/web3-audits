# Papr

## Gas Optmizations Report

### G-01 `<x> += <y>` / `<x> -= <y>` costs more gas than `<x> = <x> + <y>` / `<x> = <x> - <y>` for state variables

_There are **2** instances of this issue:_

```solidity
File: /src/PaprController.sol

info.count -= 1;
```

https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/PaprController.sol

```solidity
419: _vaultInfo[account][collateral.addr].count += 1;
```

https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/PaprController.sol

### G-02 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **6** instances of this issue:_

```solidity
File: /src/PaprController.sol

350: function setPool(address _pool) external override onlyOwner {

355: function setFundingPeriod(uint256 _fundingPeriod) external override onlyOwner {

360: function setLiquidationsLocked(bool locked) external override onlyOwner {

365: function setAllowedCollateral(IPaprController.CollateralAllowedConfig[] calldata collateralConfigs)

382: function sendPaprFromAuctionFees(address to, uint256 amount) external override onlyOwner {

386: function burnPaprFromAuctionFees(uint256 amount) external override onlyOwner {
```

https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/PaprController.sol
