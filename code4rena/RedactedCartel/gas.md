# Redacted Cartel

## Gas Optimizations Report

### G-01 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **24** instances of this issue:_

```solidity
File: /src/PirexFees.sol

63: function setFeeRecipient(FeeRecipient f, address recipient)

83: function setTreasuryFeePercent(uint8 _treasuryFeePercent)
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexFees.sol

```solidity
File: /src/PirexGmx.sol

300: function setFee(Fees f, uint256 fee) external onlyOwner {

313: function setContract(Contracts c, address contractAddress)

862: function setDelegationSpace(

884: function setVoteDelegate(address voteDelegate) external onlyOwner {

895: function clearVoteDelegate() public onlyOwner {

909: function setPauseState(bool state) external onlyOwner {

921: function initiateMigration(address newContract)

956: function completeMigration(address oldContract)
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexGmx.sol

```solidity
File: /src/PirexRewards.sol

93: function setProducer(address _producer) external onlyOwner {

151: function addRewardToken(ERC20 producerToken, ERC20 rewardToken)

179: function removeRewardToken(ERC20 producerToken, uint256 removalIndex)

432: function setRewardRecipientPrivileged(

461: function unsetRewardRecipientPrivileged(
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexRewards.sol

```solidity
File: /src/vaults/AutoPxGlp.sol

94: function setWithdrawalPenalty(uint256 penalty) external onlyOwner {

106: function setPlatformFee(uint256 fee) external onlyOwner {

118: function setCompoundIncentive(uint256 incentive) external onlyOwner {

130: function setPlatform(address _platform) external onlyOwner {
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/vaults/AutoPxGlp.sol

```solidity
File: /src/vaults/AutoPxGmx.sol

104: function setPoolFee(uint24 _poolFee) external onlyOwner {

116: function setWithdrawalPenalty(uint256 penalty) external onlyOwner {

128: function setPlatformFee(uint256 fee) external onlyOwner {

140: function setCompoundIncentive(uint256 incentive) external onlyOwner {

152: function setPlatform(address _platform) external onlyOwner {
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/vaults/AutoPxGmx.sol

### G-02 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **6** instances of this issue:_

```solidity
File: /src/PirexRewards.sol

361: producerState.rewardStates[rewardTokens[i]] += r;
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexRewards.sol

```solidity
File: /src/PxERC20.sol

85: balanceOf[msg.sender] -= amount;

90: balanceOf[to] += amount;

119: balanceOf[from] -= amount;

124: balanceOf[to] += amount;
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PxERC20.sol

```solidity
File: /src/vaults/PxGmxReward.sol

95: rewardState += rewardAmount;
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/vaults/PxGmxReward.sol

### G-03 Duplicated `require()`/`revert()` checks should be refactored to a `modifier` or `function`

_There are **23** instances of this issue:_

```solidity
File: /src/PirexGmx.sol

388: if (amount == 0) revert ZeroAmount();

432: if (amount == 0) revert ZeroAmount();

599: if (token == address(0)) revert ZeroAddress();

628: if (amount == 0) revert ZeroAmount();

727: if (token == address(0)) revert ZeroAddress();

829: if (token == address(0)) revert ZeroAddress();

830: if (amount == 0) revert ZeroAmount();
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexGmx.sol

```solidity
File: /src/PirexRewards.sol

112: if (address(producerToken) == address(0)) revert ZeroAddress();

113: if (address(rewardToken) == address(0)) revert ZeroAddress();

136: if (address(producerToken) == address(0)) revert ZeroAddress();

137: if (address(rewardToken) == address(0)) revert ZeroAddress();

155: if (address(producerToken) == address(0)) revert ZeroAddress();

156: if (address(rewardToken) == address(0)) revert ZeroAddress();

183: if (address(producerToken) == address(0)) revert ZeroAddress();

271: if (address(producerToken) == address(0)) revert ZeroAddress();

282: if (address(producerToken) == address(0)) revert ZeroAddress();

283: if (user == address(0)) revert ZeroAddress();

374: if (address(producerToken) == address(0)) revert ZeroAddress();

375: if (user == address(0)) revert ZeroAddress();

439: if (address(producerToken) == address(0)) revert ZeroAddress();

440: if (address(rewardToken) == address(0)) revert ZeroAddress();

467: if (address(producerToken) == address(0)) revert ZeroAddress();

468: if (address(rewardToken) == address(0)) revert ZeroAddress();
```

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexRewards.sol
