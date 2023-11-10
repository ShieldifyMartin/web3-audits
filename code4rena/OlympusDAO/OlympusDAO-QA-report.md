#  Olympus DAO
## Low Risk and Non-Critical Issues

### L-01 Zero-address checks are missing


_There are **2** instances of this issue:_

```solidity
File: Kernel.sol

251: executor = target_;

253: admin = target_;
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/Kernel.sol

### L-02 `safeMint()` should be used rather than `_mint()` wherever possible


_There are **1** instances of this issue:_

```solidity
File: modules/VOTES.sol

36: _mint(wallet_, amount_);
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/VOTES.sol

### N-01 Event is missing `indexed` fields


_There are **23** instances of this issue:_

```solidity
File: policies/Governance.sol

86: event ProposalSubmitted(uint256 proposalId);

87: event ProposalEndorsed(uint256 proposalId, address voter, uint256 amount);

88: event ProposalActivated(uint256 proposalId, uint256 timestamp);

89: event WalletVoted(uint256 proposalId, address voter, bool for_, uint256 userVotes);

90: event ProposalExecuted(uint256 proposalId);
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/Governance.sol

```solidity
File: policies/Heart.sol

28: event Beat(uint256 timestamp_);

29: event RewardIssued(address to_, uint256 rewardAmount_);

30: event RewardUpdated(ERC20 token_, uint256 rewardAmount_);
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/Heart.sol

```solidity
File: policies/Operator.sol

51: event CushionFactorChanged(uint32 cushionFactor_);

52: event CushionParamsChanged(uint32 duration_, uint32 debtBuffer_, uint32 depositInterval_);

53: event ReserveFactorChanged(uint32 reserveFactor_);

54: event RegenParamsChanged(uint32 wait_, uint32 threshold_, uint32 observe_);
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/Operator.sol

```solidity
File: modules/INSTR.sol

11: event InstructionsStored(uint256 instructionsId);
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/INSTR.sol

```solidity
File: modules/PRICE.sol

26: event NewObservation(uint256 timestamp_, uint256 price_, uint256 movingAverage_);

27: event MovingAverageDurationChanged(uint48 movingAverageDuration_);

28: event ObservationFrequencyChanged(uint48 observationFrequency_);
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/PRICE.sol

```solidity
File: modules/RANGE.sol

20: event WallUp(bool high_, uint256 timestamp_, uint256 capacity_);

21: event WallDown(bool high_, uint256 timestamp_, uint256 capacity_);

22: event CushionUp(bool high_, uint256 timestamp_, uint256 capacity_);

23: event CushionDown(bool high_, uint256 timestamp_);

24: event PricesChanged(uint256 wallLowPrice_, uint256 cushionLowPrice_, uint256 cushionHighPrice_, uint256 wallHighPrice_);

30: event SpreadsChanged(uint256 cushionSpread_, uint256 wallSpread_);

31: event ThresholdFactorChanged(uint256 thresholdFactor_);
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/RANGE.sol

### N-02 `constant`s should be defined rather than using magic numbers


_There are **6** instances of this issue:_

```solidity
File: policies/BondCallback.sol

71: requests = new Permissions[](4);
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/BondCallback.sol

```solidity
File: policies/Operator.sol

111: if (configParams[4] > 10000 || configParams[4] < 100) revert Operator_InvalidParams();

518: if (cushionFactor_ > 10000 || cushionFactor_ < 100) revert Operator_InvalidParams();

550: if (reserveFactor_ > 10000 || reserveFactor_ < 100) revert Operator_InvalidParams();
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/Operator.sol

```solidity
File: modules/RANGE.sol

244: if (wallSpread_ > 10000 || wallSpread_ < 100 || cushionSpread_ > 10000 ||cushionSpread_ < 100 || cushionSpread_ > wallSpread_) revert RANGE_InvalidParams();

264: if (thresholdFactor_ > 10000 || thresholdFactor_ < 100) revert RANGE_InvalidParams();
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/RANGE.sol

### N-03 Open TODOs


_There are **4** instances of this issue:_

```solidity
File: policies/TreasuryCustodian.sol

51: // TODO Currently allows anyone to revoke any approval EXCEPT activated policies.

52: // TODO must reorg policy storage to be able to check for deactivated policies.

56: // TODO Make sure `policy_` is an actual policy and not a random address.
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/TreasuryCustodian.sol

```solidity
File: policies/Operator.sol

657: /// TODO determine if this should use the last price from the MA or recalculate the current price, ideally last price is ok since it should have been just updated and should include check against secondary?
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/Operator.sol