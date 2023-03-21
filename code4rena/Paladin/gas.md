# Paladin

## Gas Optimisation Report

### G-01 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **10** instances of this issue:_

```solidity
File: /contracts/WardenPledge.sol

541: function addMultipleRewardToken(address[] calldata tokens, uint256[] calldata minRewardsPerSecond) external onlyOwner {

560: function addRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {

570: function updateRewardToken(address token, uint256 minRewardPerSecond) external onlyOwner {

585: function removeRewardToken(address token) external onlyOwner {

599: function updateChest(address chest) external onlyOwner {

612: function updateMinTargetVotes(uint256 newMinTargetVotes) external onlyOwner {

625: function updatePlatformFee(uint256 newFee) external onlyOwner {

636: function pause() external onlyOwner {

643: function unpause() external onlyOwner {

653: function recoverERC20(address token) external onlyOwner returns(bool) {
```

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol

### G-02 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **5** instances of this issue:_

```solidity
File: /contracts/WardenPledge.sol

229: if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();

380: if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();

426: if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();

429: if(newRewardPerVote <= oldRewardPerVote) revert Errors.RewardsPerVotesTooLow();

495: if(pledgeParams.endTimestamp <= block.timestamp) revert Errors.ExpiredPledge();
```

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol

### G-03 Using `bools` for storage incurs overhead

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past.

_There are **1** instances of this issue:_

```solidity
File: /contracts/WardenPledge.sol

43: bool closed;
```

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol
