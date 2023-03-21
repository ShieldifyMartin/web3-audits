# Paladin

## QA Report

### N-01 Event is missing `indexed` fields

_There are **4** instances of this issue:_

```solidity
File: /contracts/WardenPledge.sol

85: event NewPledge(

115: event ChestUpdated(address oldChest, address newChest);

117: event PlatformFeeUpdated(uint256 oldfee, uint256 newFee);

119: event MinTargetUpdated(uint256 oldMinTarget, uint256 newMinTargetVotes);
```

https://github.com/code-423n4/2022-10-paladin/blob/main/contracts/WardenPledge.sol
