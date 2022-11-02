# VTVL

## Gas Optimisation Report

### G-01 The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

_There are **1** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

353: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

### G-02 Using `> 0` costs less gas than `!= 0` when used on a `uint`

_There are **7** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

107: require(_claim.startTimestamp > 0, "NO_ACTIVE_CLAIM");

256: require(_linearVestAmount + _cliffAmount > 0, "INVALID_VESTED_AMOUNT");

257: require(_startTimestamp > 0, "INVALID_START_TIMESTAMP");

263: require(_releaseIntervalSecs > 0, "INVALID_RELEASE_INTERVAL");

272: _cliffReleaseTimestamp > 0 &&

273: _cliffAmount > 0 &&

449: require(bal > 0, "INSUFFICIENT_BALANCE");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

### G-03 It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied

_There are **3** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

27: uint112 public numTokensReservedForVesting = 0;

148: uint112 vestAmt = 0;

335: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

### G-04 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **6** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

161: vestAmt += _claim.cliffAmount;

179: vestAmt += linearVestAmount;

301: numTokensReservedForVesting += allocatedAmount;

381: usrClaim.amountWithdrawn += amountRemaining;

383: numTokensReservedForVesting -= amountRemaining;

433: numTokensReservedForVesting -= amountRemaining; // Reduces the allocation
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

### G-05 Don't compare boolean expressions to boolean literals

Use if(x)/if(!x) instead of if(x == true)/if(x == false).

_There are **1** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

111: require(_claim.isActive == true, "NO_ACTIVE_CLAIM");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

### G-06 Use custom errors rather than `revert()` / `require()` strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **23** instances of this issue:_

```solidity
File: contracts/AccessProtected.sol

40: require(admin != address(0), "INVALID_ADDRESS");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/AccessProtected.sol

```solidity
File: contracts/token/VariableSupplyERC20Token.sol

27: require(initialSupply* > 0 || maxSupply* > 0, "INVALID_AMOUNT");

37: require(account != address(0), "INVALID_ADDRESS");

41: require(amount <= mintableSupply, "INVALID_AMOUNT");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/token/VariableSupplyERC20Token.sol

```solidity
File: contracts/token/FullPremintERC20Token.sol

11: require(supply_ > 0, "NO_ZERO_MINT");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/token/FullPremintERC20Token.sol

```solidity
File: contracts/VTVLVesting.sol

82: require(address(_tokenAddress) != address(0), "INVALID_ADDRESS");

107: require(_claim.startTimestamp > 0, "NO_ACTIVE_CLAIM");

111: require(_claim.isActive == true, "NO_ACTIVE_CLAIM");

129: require(_claim.startTimestamp == 0, "CLAIM_ALREADY_EXISTS");

255: require(_recipient != address(0), "INVALID_ADDRESS");

256: require(_linearVestAmount + _cliffAmount > 0, "INVALID_VESTED_AMOUNT");

257: require(_startTimestamp > 0, "INVALID_START_TIMESTAMP");

262: require(_startTimestamp < _endTimestamp, "INVALID_END_TIMESTAMP");

263: require(_releaseIntervalSecs > 0, "INVALID_RELEASE_INTERVAL");

264: require((_endTimestamp - _startTimestamp) % _releaseIntervalSecs == 0, "INVALID_INTERVAL_LENGTH");

270: require(

295: require(tokenAddress.balanceOf(address(this)) >= numTokensReservedForVesting + allocatedAmount, "INSUFFICIENT_BALANCE");

344: require(_startTimestamps.length == length &&

373: require(allowance > usrClaim.amountWithdrawn, "NOTHING_TO_WITHDRAW");

402: require(amountRemaining >= _amountRequested, "INSUFFICIENT_BALANCE");

426: require( _claim.amountWithdrawn < finalVestAmt, "NO_UNVESTED_AMOUNT");

447: require(_otherTokenAddress != tokenAddress, "INVALID_TOKEN");

449: require(bal > 0, "INSUFFICIENT_BALANCE");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

### G-07 Splitting `require()` statements that use `&&` or `||` saves gas

_There are **3** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

270: require(

344: require(_startTimestamps.length == length &&
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

```solidity
File: contracts/token/VariableSupplyERC20Token.sol

27: require(initialSupply* > 0 || maxSupply* > 0, "INVALID_AMOUNT");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/token/VariableSupplyERC20Token.sol

### G-08 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **4** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

160: if(_referenceTs >= _claim.cliffReleaseTimestamp) {

274: _cliffReleaseTimestamp <= _startTimestamp

295: require(tokenAddress.balanceOf(address(this)) >= numTokensReservedForVesting + allocatedAmount, "INSUFFICIENT_BALANCE");

402: require(amountRemaining >= _amountRequested, "INSUFFICIENT_BALANCE");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

### G-09 Use `calldata` instead of `memory` for function parameters

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **8** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

147: function _baseVestedAmount(Claim memory _claim, uint40 _referenceTs) internal pure returns (uint112) {

334: address[] memory _recipients,

335: uint40[] memory _startTimestamps,

336: uint40[] memory _endTimestamps,

337: uint40[] memory _cliffReleaseTimestamps,

338: uint40[] memory _releaseIntervalsSecs,

339: uint112[] memory _linearVestAmounts,

340: uint112[] memory _cliffAmounts)
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

### G-10 `public` functions not called by the contract should be declared `external` instead

_There are **1** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

398: function withdrawAdmin(uint112 _amountRequested) public onlyAdmin {
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

### G-11 Functions that are access-restricted from most users may be marked as `payable`

_There are **1** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

398: function withdrawAdmin(uint112 _amountRequested) public onlyAdmin {
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol
