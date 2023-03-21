# VTVL

## QA Report

### N-01 `require()` / `revert()` statements should have descriptive reason strings

_There are **24** instances of this issue:_

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

374: require(allowance > usrClaim.amountWithdrawn, "NOTHING_TO_WITHDRAW");

402: require(amountRemaining >= _amountRequested, "INSUFFICIENT_BALANCE");

426: require( _claim.amountWithdrawn < finalVestAmt, "NO_UNVESTED_AMOUNT");

447: require(_otherTokenAddress != tokenAddress, "INVALID_TOKEN");

449: require(bal > 0, "INSUFFICIENT_BALANCE");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

```solidity
File: contracts/AccessProtected.sol

25: require(_admins[_msgSender()], "ADMIN_ACCESS_REQUIRED");

40: require(admin != address(0), "INVALID_ADDRESS");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/AccessProtected.sol

```solidity
File: contracts/token/FullPremintERC20Token.sol

11: require(supply_ > 0, "NO_ZERO_MINT");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/token/FullPremintERC20Token.sol

```solidity
File: contracts/token/VariableSupplyERC20Token.sol

27: require(initialSupply_ > 0 || maxSupply_ > 0, "INVALID_AMOUNT");

37: require(account != address(0), "INVALID_ADDRESS");

41: require(amount <= mintableSupply, "INVALID_AMOUNT");
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/token/VariableSupplyERC20Token.sol

### N-02 `public` functions not called by the contract should be declared `external` instead

_There are **2** instances of this issue:_

```solidity
File: contracts/VTVLVesting.sol

398: function withdrawAdmin(uint112 _amountRequested) public onlyAdmin {
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/VTVLVesting.sol

```solidity
File: contracts/AccessProtected.sol

39: function setAdmin(address admin, bool isEnabled) public onlyAdmin {
```

https://github.com/code-423n4/2022-09-vtvl/blob/main/contracts/AccessProtected.sol
