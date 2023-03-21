# ParaSpace

## Gas Optimizations Report

### G-01 Use elementary types or a user-defined type instead of a `struct` that has only one member

_There are **1** instances of this issue:_

```solidity
File: /protocol/tokenization/libraries/ApeStakingLogic.sol

26: struct APEStakingParameter {
    uint256 unstakeIncentive;
}
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol

### G-02 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

```solidity
File: /misc/UniswapV3OracleWrapper.sol

File: paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

File: paraspace-core/contracts/protocol/pool/PoolApeStaking.sol

File: paraspace-core/contracts/protocol/tokenization/libraries/ApeStakingLogic.sol
```

### G-03 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

```solidity
File: paraspace-core/contracts/misc/NFTFloorOracle.sol

File: paraspace-core/contracts/misc/ParaSpaceOracle.sol

File: paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

File: paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

File: paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol

File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

File: paraspace-core/contracts/protocol/pool/DefaultReserveAuctionStrategy.sol

File: paraspace-core/contracts/protocol/tokenization/NTokenMAYC.sol

File: paraspace-core/contracts/protocol/tokenization/NTokenBAYC.sol

File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol

FIle: paraspace-core/contracts/protocol/libraries/logic/GenericLogic.sol

File: paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol
```

### G-04 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

```solidity
File: paraspace-core/contracts/ui/WPunkGateway.sol

File: paraspace-core/contracts/protocol/configuration/PoolAddressesProvider.sol
```

### G-05 Splitting `require()` statements that use && saves gas

Instead of using `&&` on single require check using two require checks can save gas

```solidity
File: paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

File: paraspace-core/contracts/protocol/pool/PoolParameters.sol

File: paraspace-core/contracts/protocol/libraries/logic/ValidationLogic.sol

File: paraspace-core/contracts/protocol/tokenization/libraries/MintableERC721Logic.sol
```
