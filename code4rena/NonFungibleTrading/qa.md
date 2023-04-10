# Non Fungible Trading

## QA Report

### L-01 Modifiers shouldn’t update state

_There are **4** instances of this issue:_

```solidity
File: /contracts/Exchange.sol

41: remainingETH = msg.value;

42: isInternal = true;

44: remainingETH = 0;

45: isInternal = false;
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Exchange.sol

### N-01 Event is missing `indexed` fields

Each `event` should use three `indexed` fields if there are three or more fields

_There are **4** instances of this issue:_

```solidity
File: /contracts/Exchange.sol

53: event Opened();

54: event Closed();

99: event OrderCancelled(bytes32 hash);

105: event NewBlockRange(uint256 blockRange);
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Exchange.sol

### N-02 `require()`/`revert()` statements should have descriptive reason strings

_There are **7** instances of this issue:_

```solidity
File: /contracts/Pool.sol

45: require(_balances[msg.sender] >= amount);

48: require(success);

71: require(_balances[from] >= amount);

72: require(to != address(0));
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Pool.sol

```solidity
File: /contracts/Exchange.sol

240: require(sell.order.side == Side.Sell);

291: require(msg.sender == order.trader);

573: require(remainingETH >= price);
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Exchange.sol

### N-03 `public` functions not called by the contract should be declared `external` instead

_There are **4** instances of this issue:_

```solidity
File: /contracts/Pool.sol

44: function withdraw(uint256 amount) public {

58: function transferFrom(address from, address to, uint256 amount)

79: function balanceOf(address user) public view returns (uint256) {

83: function totalSupply() public view returns (uint256) {
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Pool.sol

### N-04 Open TODOs

_There are **1** instances of this issue:_

```solidity
File: /contracts/Pool.sol

18: // TODO: set proper address before deployment
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Pool.sol

### N-05 Not using the named `return` variables anywhere in the `function` is confusing

Consider changing the variable to be an unnamed one

_There are **1** instances of this issue:_

```solidity
File: /contracts/Exchange.sol

540: returns (uint256 price, uint256 tokenId, uint256 amount, AssetType assetType)
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Exchange.sol
