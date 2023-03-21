# LSD Network

## Gas Optimizations Report

### G-01 Duplicated `require()`/`revert()` checks should be refactored to a modifier or function

_There are **14** instances of this issue:_

```solidity
File: /contracts/liquid-staking/GiantLP.sol

30: require(msg.sender == pool, "Only pool");

35: require(msg.sender == pool, "Only pool");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantLP.sol

```solidity
File: /contracts/liquid-staking/GiantPoolBase.sol

53: require(_amount >= MIN_STAKING_AMOUNT, "Invalid amount");

94: require(_amount >= MIN_STAKING_AMOUNT, "Invalid amount");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantPoolBase.sol

```solidity
File: /contracts/liquid-staking/SavETHVault.sol

101: require(numOfTokens > 0, "Empty arrays");

114: require(numOfTokens > 0, "Empty arrays");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/SavETHVault.sol

```solidity
File: /contracts/liquid-staking/GiantMevAndFeesPool.sol

147: require(msg.sender == address(lpTokenETH), "Caller is not giant LP");

171: require(msg.sender == address(lpTokenETH), "Caller is not giant LP");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantMevAndFeesPool.sol

```solidity
File: /contracts/liquid-staking/StakingFundsVault.sol

150: require(numOfTokens > 0, "Empty arrays");

164: require(numOfTokens > 0, "Empty arrays");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/StakingFundsVault.sol

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

279: require(_nodeRunner != address(0), "Zero address");

294: require(smartWallet != address(0), "No smart wallet");

312: require(smartWallet != address(0), "No smart wallet");

685: require(_nodeRunner != address(0), "Zero address");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol

### G-02 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **27** instances of this issue:_

```solidity
File: /contracts/liquid-staking/GiantPoolBase.sol

35: require(msg.value >= MIN_STAKING_AMOUNT, "Minimum not supplied");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantPoolBase.sol

```solidity
File: /contracts/liquid-staking/GiantSavETHVaultPool.sol

92: require(lpTokenETH.balanceOf(msg.sender) >= amount, "User does not own enough LP");

127: require(lpTokenETH.balanceOf(msg.sender) >= 0.5 ether, "No common interest");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantSavETHVaultPool.sol

```solidity
File: /contracts/liquid-staking/SavETHVault.sol

127: require(_amount >= MIN_STAKING_AMOUNT, "Amount cannot be zero");

128: require(_amount <= _lpToken.balanceOf(msg.sender), "Not enough balance");

161: require(dETHBalance >= 24 ether, "Nothing to withdraw");

204: require(_amount >= 24 ether, "Amount cannot be less than 24 ether");

205: require(address(this).balance >= _amount, "Insufficient withdrawal amount");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/SavETHVault.sol

```solidity
File: /contracts/liquid-staking/GiantMevAndFeesPool.sol

116: require(lpTokenETH.balanceOf(msg.sender) >= 0.5 ether, "No common interest");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantMevAndFeesPool.sol

```solidity
File: /contracts/liquid-staking/StakingFundsVault.sol

175: require(_amount >= MIN_STAKING_AMOUNT, "Amount cannot be zero");

176: require(_amount <= _lpToken.balanceOf(msg.sender), "Not enough balance");

240: require(_amount >= 4 ether, "Amount cannot be less than 4 ether");

241: require(_amount <= address(this).balance, "Not enough ETH to withdraw");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/StakingFundsVault.sol

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

256: require(bytes(_newTicker).length >= 3, "String must be 3-5 characters long");

257: require(bytes(_newTicker).length <= 5, "String must be 3-5 characters long");

333: require(associatedSmartWallet.balance >= 4 ether, "Insufficient balance");

432: require(len >= 1, "No value provided");

662: require(bytes(_stakehouseTicker).length >= 3, "String must be 3-5 characters long");

663: require(bytes(_stakehouseTicker).length <= 5, "String must be 3-5 characters long");

936: require(associatedSmartWallet.balance >= 4 ether, "Smart wallet balance must be at least 4 ether");

949: require(_commissionPercentage <= MODULO, "Invalid commission");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol

```solidity
File: /contracts/liquid-staking/ETHPoolLPFactory.sol

80: require(_amount >= MIN_STAKING_AMOUNT, "Amount cannot be zero");

81: require(_amount <= _oldLPToken.balanceOf(msg.sender), "Not enough balance");

83: require(_amount + _newLPToken.totalSupply() <= 24 ether, "Not enough mintable tokens");

111: require(_amount >= MIN_STAKING_AMOUNT, "Min amount not reached");

122:  require(lpToken.totalSupply() + _amount <= maxStakingAmountPerValidator, "Amount exceeds the staking limit for the validator");

130: require(_amount <= maxStakingAmountPerValidator, "Amount exceeds the staking limit for the validator");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/ETHPoolLPFactory.sol

### G-03 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **31** instances of this issue:_

```solidity
File: /contracts/liquid-staking/GiantPoolBase.sol

39: idleETH += msg.value;

57: idleETH -= _amount;
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantPoolBase.sol

```solidity
File: /contracts/liquid-staking/GiantSavETHVaultPool.sol

46: idleETH -= transactionAmount;
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantSavETHVaultPool.sol

```solidity
File: /contracts/liquid-staking/SavETHVault.sol

71: totalAmount += amount;
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/SavETHVault.sol

```solidity
File: /contracts/liquid-staking/GiantMevAndFeesPool.sol

40: idleETH -= _ETHTransactionAmounts[i];
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantMevAndFeesPool.sol

```solidity
File: /contracts/liquid-staking/StakingFundsVault.sol

58: totalShares += 4 ether;

97: totalAmount += amount;

278: totalAccumulated += previewAccumulatedETH(_user, token);

287: totalAccumulated += previewAccumulatedETH(_user, _token[i]);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/StakingFundsVault.sol

```solidity
File: /contracts/syndicate/Syndicate.sol

185: accumulatedETHPerFreeFloatingShare += _calculateNewAccumulatedETHPerFreeFloatingShare(freeFloatingUnprocessed);

190: accumulatedETHPerCollateralizedSlotPerKnot += collateralizedUnprocessed;

225: totalFreeFloatingShares += _sETHAmount;

226: sETHTotalStakeForKnot[_blsPubKey] += _sETHAmount;

227: sETHStakedBalanceForKnot[_blsPubKey][_onBehalfOf] += _sETHAmount;

269: totalFreeFloatingShares -= _sETHAmount;

272: sETHTotalStakeForKnot[_blsPubKey] -= _sETHAmount;

273: sETHStakedBalanceForKnot[_blsPubKey][msg.sender] -= _sETHAmount;

317: totalClaimed += unclaimedUserShare;

430: currentAccrued +=

511: accruedEarningPerCollateralizedSlotOwnerOfKnot[_blsPubKey][collateralizedOwnerAtIndex] += unprocessedETHForCurrentKnot;

521: accruedEarningPerCollateralizedSlotOwnerOfKnot[_blsPubKey][collateralizedOwnerAtIndex] +=

558: numberOfRegisteredKnots += knotsToRegister;

621: totalFreeFloatingShares -= sETHTotalStakeForKnot[_blsPublicKey];

624: numberOfRegisteredKnots -= 1;

658: totalClaimed += unclaimedUserShare;
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/syndicate/Syndicate.sol

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

615: stakedKnotsOfSmartWallet[smartWallet] -= 1;

770: stakedKnotsOfSmartWallet[smartWallet] += 1;

782: numberOfKnots += 1;

839: numberOfKnots += 1;
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol

```solidity
File: /contracts/liquid-staking/SyndicateRewardsProcessor.sol

65: totalClaimed += due;

85: accumulatedETHPerLPShare += (unprocessed * PRECISION) / _numOfShares;
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/SyndicateRewardsProcessor.sol

### G-04 `public` functions not called by the contract should be declared `external` instead

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 \* <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution.

_There are **8** instances of this issue:_

```solidity
File: /contracts/liquid-staking/GiantSavETHVaultPool.sol

29: function batchDepositETHForStaking(
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantSavETHVaultPool.sol

```solidity
File: /contracts/smart-wallet/OwnableSmartWallet.sol

94: function transferOwnership(address newOwner)
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/smart-wallet/OwnableSmartWallet.sol

```solidity
File: /contracts/liquid-staking/SavETHVault.sol

83: function depositETHForStaking(bytes calldata _blsPublicKeyOfKnot, uint256 _amount) public payable returns (uint256) {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/SavETHVault.sol

```solidity
File: /contracts/liquid-staking/GiantMevAndFeesPool.sol

176: function totalRewardsReceived() public view override returns (uint256) {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantMevAndFeesPool.sol

```solidity
File: /contracts/liquid-staking/StakingFundsVault.sol

113: function depositETHForStaking(bytes calldata _blsPublicKeyOfKnot, uint256 _amount) public payable returns (uint256) {

239: function withdrawETH(address _wallet, uint256 _amount) public onlyManager nonReentrant returns (uint256) {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/StakingFundsVault.sol

```solidity
File: /contracts/syndicate/Syndicate.sol

285: function claimAsStaker(address _recipient, bytes[] calldata _blsPubKeys) public nonReentrant {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/syndicate/Syndicate.sol

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

514: function isKnotDeregistered(bytes calldata _blsPublicKey) public view returns (bool) {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol

### G-05 Don't compare `boolean` expressions to `boolean` literals

Use if(x)/if(!x) instead of if(x == true)/if(x == false).

_There are **17** instances of this issue:_

```solidity
File: /contracts/liquid-staking/GiantSavETHVaultPool.sol

150: vault.isDETHReadyForWithdrawal(address(_lpTokens[i][j])) == false,
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantSavETHVaultPool.sol

```solidity
File: /contracts/liquid-staking/SavETHVault.sol

64: require(liquidStakingManager.isBLSPublicKeyBanned(_blsPublicKeyOfKnots[i]) == false, "BLS public key is not part of LSD network");

84: require(liquidStakingManager.isBLSPublicKeyBanned(_blsPublicKeyOfKnot) == false, "BLS public key is banned or not a part of LSD network");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/SavETHVault.sol

```solidity
File: /contracts/liquid-staking/StakingFundsVault.sol

79: require(liquidStakingNetworkManager.isBLSPublicKeyBanned(_blsPublicKeyOfKnots[i]) == false, "BLS public key is not part of LSD network");

114: require(liquidStakingNetworkManager.isBLSPublicKeyBanned(_blsPublicKeyOfKnot) == false, "BLS public key is banned or not a part of LSD network");

205: liquidStakingNetworkManager.isBLSPublicKeyBanned(_blsPubKeys[i]) == false,
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/StakingFundsVault.sol

```solidity
File: /contracts/syndicate/Syndicate.sol

611: if (isKnotRegistered[_blsPublicKey] == false) revert KnotIsNotRegisteredWithSyndicate();

612: if (isNoLongerPartOfSyndicate[_blsPublicKey] == true) revert KnotHasAlreadyBeenDeRegistered();
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/syndicate/Syndicate.sol

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

328: require(isBLSPublicKeyBanned(_blsPublicKeyOfKnot) == false, "BLS public key has already withdrawn or not a part of LSD network");

332: require(isNodeRunnerBanned(nodeRunnerOfSmartWallet[associatedSmartWallet]) == false, "Node runner is banned from LSD network");

393: require(isBLSPublicKeyBanned(_blsPubKeys[i]) == false, "BLS public key is banned or not a part of LSD network");

436: require(_isNodeRunnerValid(msg.sender) == true, "Unrecognised node runner");

437: require(isNodeRunnerBanned(msg.sender) == false, "Node runner is banned from LSD network");

469: require(isBLSPublicKeyPartOfLSDNetwork(_blsPublicKey) == false, "BLS public key is banned or not a part of LSD network");

541: require(isBLSPublicKeyBanned(blsPubKey) == false, "BLS public key is banned or not a part of LSD network");

589: require(isBLSPublicKeyBanned(_blsPublicKeyOfKnots[i]) == false, "BLS public key is banned or not a part of LSD network");

688: require(isNodeRunnerWhitelisted[_nodeRunner] == true, "Invalid node runner");
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol

### G-06 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **16** instances of this issue:_

```solidity
File: /contracts/smart-wallet/OwnableSmartWallet.sol

114: function setApproval(address to, bool status) external onlyOwner override {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/smart-wallet/OwnableSmartWallet.sol

```solidity
File: /contracts/liquid-staking/SavETHVault.sol

209: function withdrawETHForStaking(
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/SavETHVault.sol

```solidity
File: /contracts/liquid-staking/StakingFundsVault.sol

239: function withdrawETH(address _wallet, uint256 _amount) public onlyManager nonReentrant returns (uint256) {

259: ) external onlyManager nonReentrant {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/StakingFundsVault.sol

```solidity
File: /contracts/syndicate/Syndicate.sol

147: ) external onlyOwner {

154: function deRegisterKnots(bytes[] calldata _blsPublicKeys) external onlyOwner {

161: function addPriorityStakers(address[] calldata _priorityStakers) external onlyOwner {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/syndicate/Syndicate.sol

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

207: ) external payable onlyDAO {

218: function deRegisterKnotFromSyndicate(bytes[] calldata _blsPublicKeys) external onlyDAO {

230: ) external onlyDAO {

239: function updateDAOAddress(address _newAddress) external onlyDAO {

249: function updateDAORevenueCommission(uint256 _commissionPercentage) external onlyDAO {

255: function updateTicker(string calldata _newTicker) external onlyDAO {

267: function updateWhitelisting(bool _changeWhitelist) external onlyDAO returns (bool) {

278: function updateNodeRunnerWhitelistStatus(address _nodeRunner, bool isWhitelisted) external onlyDAO {

308: function rotateEOARepresentativeOfNodeRunner(address _nodeRunner, address _newRepresentative) external onlyDAO {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol

### G-07 Splitting `require()` statements that use `&&` saves gas

Instead of using && on single require check using two require checks can save gas

_There are **4** instances of this issue:_

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

357: require(_new != address(0) && _current != _new, "New is zero or current");

371: if (msg.sender == dao && _wasPreviousNodeRunnerMalicious) {

700: if(!_isEnabled && smartWalletRepresentative[_smartWallet] != address(0)) {

717: else if(_isEnabled && smartWalletRepresentative[_smartWallet] == address(0)) {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol

### G-08 Not using the named `return` variables when a function returns wastes deployment gas

_There are **1** instances of this issue:_

```solidity
File: /contracts/liquid-staking/LiquidStakingManager.sol

921: function _calculateCommission(uint256 _received) internal virtual view returns (uint256 _nodeRunner, uint256 _dao) {
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/LiquidStakingManager.sol
