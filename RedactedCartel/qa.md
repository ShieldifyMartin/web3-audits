# Redacted Cartel

## QA Report

### L-01 `emit` function called early

_There are **5** instances of this issue:_

```solidity
File: /src/PirexFees.sol

69: emit SetFeeRecipient(f, recipient);

106: emit DistributeFees(
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexFees.sol

```solidity
File: /src/PirexGmx.sol

319: emit SetContract(c, contractAddress);

896: emit ClearVoteDelegate();
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexGmx.sol

```solidity
File: /src/vaults/AutoPxGmx.sol

334: emit Withdraw(msg.sender, receiver, owner, assets, shares);
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/vaults/AutoPxGmx.sol

### L-02 `require()` should be used instead of `assert()`

_There are **1** instances of this issue:_

```solidity
File: /src/PirexGmx.sol

225: assert(feeAmount + postFeeAmount == assets);
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexGmx.sol

### L-03 Perform `msg.sender` balance and `to` `0x0` address checks

_There are **2** instances of this issue:_

```solidity
File: /src/PxERC20.sol

80: function transfer(address to, uint256 amount)

109: function transferFrom(
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PxERC20.sol

### L-04 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **3** instances of this issue:_

```solidity
File: /src/vaults/AutoPxGlp.sol

File: /src/vaults/AutoPxGmx.sol

File: /src/vaults/PirexERC4626.sol
```

### N-01 Event is missing `indexed` fields

Each `event` should use three `indexed` fields if there are three or more fields

_There are **21** instances of this issue:_

```solidity
File: /src/PirexFees.sol

34: event SetFeeRecipient(FeeRecipient f, address recipient);

35: event SetTreasuryFeePercent(uint8 \_treasuryFeePercent);
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexFees.sol

```solidity
File: /src/PirexGmx.sol

125: event ClaimRewards(

140: event InitiateMigration(address newContract);

141: event CompleteMigration(address oldContract);

142: event SetDelegationSpace(string delegationSpace, bool shouldClear);

143: event SetVoteDelegate(address voteDelegate);

144: event ClearVoteDelegate();
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexGmx.sol

```solidity
File: /src/PirexRewards.sol

33: event SetProducer(address producer);

63: event Harvest(
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexRewards.sol

```solidity
File: /src/vaults/AutoPxGlp.sol

35: event WithdrawalPenaltyUpdated(uint256 penalty);

36: event PlatformFeeUpdated(uint256 fee);

37: event CompoundIncentiveUpdated(uint256 incentive);

38: event PlatformUpdated(address _platform);
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/vaults/AutoPxGlp.sol

```solidity
File: /src/vaults/AutoPxGmx.sol

39: event PoolFeeUpdated(uint24 _poolFee);

40: event WithdrawalPenaltyUpdated(uint256 penalty);

41: event PlatformFeeUpdated(uint256 fee);

42: event CompoundIncentiveUpdated(uint256 incentive);

43: event PlatformUpdated(address _platform);
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/vaults/AutoPxGmx.sol

```solidity
File: /src/vaults/PxGmxReward.sol

21: event GlobalAccrue(uint256 lastUpdate, uint256 lastSupply, uint256 rewards);

28: event Harvest(uint256 rewardAmount);
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/vaults/PxGmxReward.sol

### N-02 Not using the named `return` variables anywhere in the `function` is confusing

Consider changing the variable to be an unnamed one

_There are **3** instances of this issue:_

```solidity
File: /src/PirexRewards.sol

211: uint256 lastUpdate,

212: uint256 lastBalance,

213: uint256 rewards
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexRewards.sol
