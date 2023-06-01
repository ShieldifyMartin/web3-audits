# PrePo

## Gas Optimizations Report

### G-01 Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

It is expected that the value should be converted into a constant value at compile time. But actually the expression is re-calculated each time the constant is referenced.

_There are **37** instances of this issue:_

```solidity
File: /apps/smart-contracts/core/contracts/Collateral.sol

bytes32 public constant MANAGER_WITHDRAW_ROLE = keccak256("Collateral_managerWithdraw(uint256)");

bytes32 public constant SET_MANAGER_ROLE = keccak256("Collateral_setManager(address)");

bytes32 public constant SET_DEPOSIT_FEE_ROLE = keccak256("Collateral_setDepositFee(uint256)");

bytes32 public constant SET_WITHDRAW_FEE_ROLE = keccak256("Collateral_setWithdrawFee(uint256)");

bytes32 public constant SET_DEPOSIT_HOOK_ROLE = keccak256("Collateral_setDepositHook(ICollateralHook)");

bytes32 public constant SET_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setWithdrawHook(ICollateralHook)");

bytes32 public constant SET_MANAGER_WITHDRAW_HOOK_ROLE = keccak256("Collateral_setManagerWithdrawHook(ICollateralHook)");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol

```solidity
File: /apps/smart-contracts/core/contracts/DepositHook.sol

17: bytes32 public constant SET_COLLATERAL_ROLE = keccak256("DepositHook_setCollateral(address)");

18: bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("DepositHook_setDepositRecord(address)");

19: bytes32 public constant SET_DEPOSITS_ALLOWED_ROLE = keccak256("DepositHook_setDepositsAllowed(bool)");

20: bytes32 public constant SET_ACCOUNT_LIST_ROLE = keccak256("DepositHook_setAccountList(IAccountList)");

21: bytes32 public constant SET_REQUIRED_SCORE_ROLE = keccak256("DepositHook_setRequiredScore(uint256)");

22: bytes32 public constant SET_COLLECTION_SCORES_ROLE = keccak256("DepositHook_setCollectionScores(IERC721[],uint256[])");

23: bytes32 public constant REMOVE_COLLECTIONS_ROLE = keccak256("DepositHook_removeCollections(IERC721[])");

24: bytes32 public constant SET_TREASURY_ROLE = keccak256("DepositHook_setTreasury(address)");

25: bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("DepositHook_setTokenSender(ITokenSender)");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol

```solidity
File: /apps/smart-contracts/core/contracts/DepositRecord.sol

14: bytes32 public constant SET_GLOBAL_NET_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setGlobalNetDepositCap(uint256)");

15: bytes32 public constant SET_USER_DEPOSIT_CAP_ROLE = keccak256("DepositRecord_setUserDepositCap(uint256)");

16: bytes32 public constant SET_ALLOWED_HOOK_ROLE = keccak256("DepositRecord_setAllowedHook(address)");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol

```solidity
File: /apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

bytes32 public constant SET_COLLATERAL_ROLE = keccak256("ManagerWithdrawHook_setCollateral(address)");

bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("ManagerWithdrawHook_setDepositRecord(address)");

bytes32 public constant SET_MIN_RESERVE_PERCENTAGE_ROLE = keccak256("ManagerWithdrawHook_setMinReservePercentage(uint256)");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

```solidity
File: /apps/smart-contracts/core/contracts/TokenSender.sol

26: bytes32 public constant SET_PRICE_ROLE = keccak256("TokenSender_setPrice(IUintValue)");

27: bytes32 public constant SET_PRICE_MULTIPLIER_ROLE = keccak256("TokenSender_setPriceMultiplier(uint256)");

28: bytes32 public constant SET_SCALED_PRICE_LOWER_BOUND_ROLE = keccak256("TokenSender_setScaledPriceLowerBound(uint256)");

29: bytes32 public constant SET_ALLOWED_MSG_SENDERS_ROLE = keccak256("TokenSender_setAllowedMsgSenders(IAccountList)");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/TokenSender.sol

```solidity
File: /apps/smart-contracts/core/contracts/WithdrawHook.sol

bytes32 public constant SET_COLLATERAL_ROLE = keccak256("WithdrawHook_setCollateral(address)");

bytes32 public constant SET_DEPOSIT_RECORD_ROLE = keccak256("WithdrawHook_setDepositRecord(address)");

bytes32 public constant SET_WITHDRAWALS_ALLOWED_ROLE = keccak256("WithdrawHook_setWithdrawalsAllowed(bool)");

bytes32 public constant SET_GLOBAL_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setGlobalPeriodLength(uint256)");

bytes32 public constant SET_USER_PERIOD_LENGTH_ROLE = keccak256("WithdrawHook_setUserPeriodLength(uint256)");

bytes32 public constant SET_GLOBAL_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setGlobalWithdrawLimitPerPeriod(uint256)");

bytes32 public constant SET_USER_WITHDRAW_LIMIT_PER_PERIOD_ROLE = keccak256("WithdrawHook_setUserWithdrawLimitPerPeriod(uint256)");

bytes32 public constant SET_TREASURY_ROLE = keccak256("WithdrawHook_setTreasury(address)");

bytes32 public constant SET_TOKEN_SENDER_ROLE = keccak256("WithdrawHook_setTokenSender(ITokenSender)");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol

### G-02 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **5** instances of this issue:_

```solidity
File: /packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol

60: score += IERC721(collection).balanceOf(account) > 0 ? collectionScore : 0;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/packages/prepo-shared-contracts/contracts/NFTScoreRequirement.sol

```solidity
File: /apps/smart-contracts/core/contracts/DepositRecord.sol

31: globalNetDepositAmount += _amount;

32: userToDeposits[_sender] += _amount;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol

```solidity
File: /apps/smart-contracts/core/contracts/WithdrawHook.sol

64: globalAmountWithdrawnThisPeriod += _amountBeforeFee;

71: userToAmountWithdrawnThisPeriod[_sender] += _amountBeforeFee;
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol

### G-03 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **6** instances of this issue:_

```solidity
File: /apps/smart-contracts/core/contracts/Collateral.sol

85: function setManager(address _newManager) external override onlyRole(SET_MANAGER_ROLE) {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol

```solidity
File: /apps/smart-contracts/core/contracts/LongShortToken.sol

11: function mint(address _recipient, uint256 _amount) external onlyOwner { _mint(_recipient, _amount); }
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/LongShortToken.sol

```solidity
File: /apps/smart-contracts/core/contracts/MintHook.sol

18: function setAllowedMsgSenders(IAccountList allowedMsgSenders) public virtual override onlyOwner { super.setAllowedMsgSenders(allowedMsgSenders); }

20: function setAccountList(IAccountList accountList) public virtual override onlyOwner { super.setAccountList(accountList); }
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/MintHook.sol

```solidity
File: /apps/smart-contracts/core/contracts/PrePOMarketFactory.sol

22: function createMarket(string memory _tokenNameSuffix, string memory _tokenSymbolSuffix, bytes32 longTokenSalt, bytes32 shortTokenSalt, address _governance, address _collateral, uint256 _floorLongPrice, uint256 _ceilingLongPrice, uint256 _floorValuation, uint256 _ceilingValuation, uint256 _expiryTime) external override onlyOwner nonReentrant {

36: function setCollateralValidity(address _collateral, bool _validity) external override onlyOwner {
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarketFactory.sol

### G-04 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **15** instances of this issue:_

```solidity
File: /apps/smart-contracts/core/contracts/Collateral.sol

91: require(_newDepositFee <= FEE_LIMIT, "exceeds fee limit");

97: require(_newWithdrawFee <= FEE_LIMIT, "exceeds fee limit");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/Collateral.sol

```solidity
File: /apps/smart-contracts/core/contracts/DepositRecord.sol

29: require(_amount + globalNetDepositAmount <= globalNetDepositCap, "Global deposit cap exceeded");

30: require(_amount + userToDeposits[_sender] <= userDepositCap, "User deposit cap exceeded");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositRecord.sol

```solidity
File: /apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

30: require(_newMinReservePercentage <= PERCENT_DENOMINATOR, ">100%");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/ManagerWithdrawHook.sol

```solidity
File: /apps/smart-contracts/core/contracts/PrePOMarket.sol

45: require(_ceilingLongPayout <= MAX_PAYOUT, "Ceiling cannot exceed 1");

67: require(collateral.balanceOf(msg.sender) >= _amount, "Insufficient collateral");

77: require(longToken.balanceOf(msg.sender) >= _longAmount, "Insufficient long tokens");

78: require(shortToken.balanceOf(msg.sender) >= _shortAmount, "Insufficient short tokens");

81: if (finalLongPayout <= MAX_PAYOUT) {

120: require(_finalLongPayout >= floorLongPayout, "Payout cannot be below floor");

121: require(_finalLongPayout <= ceilingLongPayout, "Payout cannot exceed ceiling");

127: require(_redemptionFee <= FEE_LIMIT, "Exceeds fee limit");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/PrePOMarket.sol

```solidity
File: /apps/smart-contracts/core/contracts/WithdrawHook.sol

63: require(globalAmountWithdrawnThisPeriod + _amountBeforeFee <= globalWithdrawLimitPerPeriod, "global withdraw limit exceeded");

70: require(userToAmountWithdrawnThisPeriod[_sender] + _amountBeforeFee <= userWithdrawLimitPerPeriod, "user withdraw limit exceeded");
```

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol
