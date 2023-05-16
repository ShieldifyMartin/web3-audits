# PoolTogether

## Gas Optimizations Report

### G-01 Splitting `require()` statements that use `&&` saves gas

Instead of using `&&` on single require check using two `require` checks can save gas

_There are **1** instances of this issue:_

```solidity
File: /src/ethereum-optimism/EthereumToOptimismExecutor.sol

81: msg.sender == address(_crossDomainMessenger) &&
```

https://github.com/pooltogether/ERC5164/blob/5647bd84f2a6d1a37f41394874d567e45a97bf48/src/ethereum-optimism/EthereumToOptimismExecutor.sol
