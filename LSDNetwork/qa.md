# LSD Network

## QA Report

### L-01 Unused/empty `receive()`/`fallback()` function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds

_There are **3** instances of this issue:_

```solidity
File: /contracts/smart-wallet/OwnableSmartWallet.sol

148: receive() external payable {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/smart-wallet/OwnableSmartWallet.sol

```solidity
File: /contracts/liquid-staking/SyndicateRewardsProcessor.sol

98: receive() external payable {}
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/SyndicateRewardsProcessor.sol

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

629: receive() external payable {}
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol

### L-02 `emit` function called in wrong order

_There are **1** instances of this issue:_

```solidity
File: /contracts/liquid-staking/StakingFundsVault.sol

192: emit ETHWithdrawnByDepositor(msg.sender, _amount);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/StakingFundsVault.sol

### L-03 Return values of `approve()` not checked

Not all IERC20 implementations revert() when there’s a failure in approve(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything.

_There are **1** instances of this issue:_

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

870: sETH.approve(syndicate, (2 ** 256) - 1);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol

### N-01 Non-floating `pragma` should be used

_There are **18** instances of this issue:_

```solidity
File: /contracts/liquid-staking/OptionalGatekeeperFactory.sol

File: /contracts/liquid-staking/OptionalHouseGatekeeper.sol

File: /contracts/liquid-staking/SavETHVaultDeployer.sol

File: /contracts/liquid-staking/StakingFundsVaultDeployer.sol

File: /contracts/smart-wallet/OwnableSmartWalletFactory.sol

File: /contracts/liquid-staking/LPTokenFactory.sol

File: /contracts/liquid-staking/GiantLP.sol

File: /contracts/liquid-staking/LPToken.sol

File: /contracts/liquid-staking/GiantPoolBase.sol

File: /contracts/liquid-staking/LSDNFactory.sol

File: /contracts/liquid-staking/GiantSavETHVaultPool.sol

File: /contracts/smart-wallet/OwnableSmartWallet.sol

File: /contracts/liquid-staking/SavETHVault.sol

File: /contracts/liquid-staking/GiantMevAndFeesPool.sol

File: /contracts/liquid-staking/StakingFundsVault.sol

File: /contracts/liquid-staking/LiquidStakingManager.sol

File: /contracts/liquid-staking/SyndicateRewardsProcessor.sol

File: /contracts/liquid-staking/ETHPoolLPFactory.sol
```

### N-02 Event is missing `indexed` fields

_There are **21** instances of this issue:_

```solidity
File: /contracts/liquid-staking/SavETHVault.sol

19: event DETHRedeemed(address depositor, uint256 amount);

22: event ETHWithdrawnForStaking(address withdrawalAddress, address liquidStakingManager, uint256 amount);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/SavETHVault.sol

```solidity
File: /contracts/liquid-staking/StakingFundsVault.sol

event ETHDeposited(address sender, uint256 amount);

event ETHWithdrawn(address receiver, address admin, uint256 amount);

event ERC20Recovered(address admin, address recipient, uint256 amount);

event WETHUnwrapped(address admin, uint256 amount);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/StakingFundsVault.sol

```solidity
File: /contracts/syndicate/Syndicate.sol

39: event ContractDeployed();

42: event UpdateAccruedETH(uint256 unprocessed);

45: event CollateralizedSLOTReCalibrated(bytes BLSPubKey);

48: event KNOTRegistered(bytes BLSPubKey);

51: event KnotDeRegistered(bytes BLSPubKey);

57: event Staked(bytes BLSPubKey, uint256 amount);

60: event UnStaked(bytes BLSPubKey, uint256 amount);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/syndicate/Syndicate.sol

```solidity
File: /contracts/liquid-staking/ETHPoolLPFactory.sol

16: event ETHWithdrawnByDepositor(address depositor, uint256 amount);

19: event LPTokenBurnt(bytes blsPublicKeyOfKnot, address token, address depositor, uint256 amount);

22: event NewLPTokenIssued(bytes blsPublicKeyOfKnot, address token, address firstDepositor, uint256 amount);

25: event LPTokenMinted(bytes blsPublicKeyOfKnot, address token, address depositor, uint256 amount);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/ETHPoolLPFactory.sol

```solidity
File: /contracts/liquid-staking/SyndicateRewardsProcessor.sol

9: event ETHReceived(uint256 amount);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/SyndicateRewardsProcessor.sol

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

57: event StakehouseJoined(bytes blsPubKey);

69: event NetworkTickerUpdated(string newTicker);

84: event DAOCommissionUpdated(uint256 old, uint256 newCommission);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol

### N-03 Open TODOs

_There are **1** instances of this issue:_

```solidity
File: /contracts/syndicate/Syndicate.sol

195: // todo - check else case for any ETH lost
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/syndicate/Syndicate.sol

### N-04 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

_There are **1** instances of this issue:_

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

870: sETH.approve(syndicate, (2 ** 256) - 1);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol
