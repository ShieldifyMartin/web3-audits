# Non Fungible Trading

## Gas Optimizations Report

### G-01 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **6** instances of this issue:_

```solidity
File: /contracts/Pool.sol

45: require(_balances[msg.sender] >= amount);

71: require(_balances[from] >= amount);
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Pool.sol

```solidity
File: /contracts/Exchange.sol

274: sell.order.listingTime <= buy.order.listingTime ? sell.order.trader : buy.order.trader,

543: if (sell.listingTime <= buy.listingTime) {

573: require(remainingETH >= price);

604: require(totalFee <= price, "Total amount of fees are more than the price");
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Exchange.sol

### G-02 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **7** instances of this issue:_

```solidity
File: /contracts/Pool.sol

36: _balances[msg.sender] += msg.value;

46: _balances[msg.sender] -= amount;

73: _balances[from] -= amount;

74: _balances[to] += amount;
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Pool.sol

```solidity
File: /contracts/Exchange.sol

316: nonces[msg.sender] += 1;

574: remainingETH -= price;

601: totalFee += fee;
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Exchange.sol

### G-03 Public functions not called by the contract should be declared `external` instead

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 \* <mem_array>.length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution.

_There are **4** instances of this issue:_

```solidity
File: /contracts/Pool.sol

44: function withdraw(uint256 amount) public {

58: function transferFrom(address from, address to, uint256 amount)

79: function balanceOf(address user) public view returns (uint256) {

83: function totalSupply() public view returns (uint256) {
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Pool.sol

### G-04 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **7** instances of this issue:_

```solidity
File: /contracts/Exchange.sol

56: function open() external onlyOwner {

60: function close() external onlyOwner {

66: function _authorizeUpgrade(address) internal override onlyOwner {}

323: function setExecutionDelegate(IExecutionDelegate _executionDelegate)

332: function setPolicyManager(IPolicyManager _policyManager)

341: function setOracle(address _oracle)

350: function setBlockRange(uint256 _blockRange)
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Exchange.sol

### G-05 Not using the named `return` variables when a `function` `returns` wastes deployment gas

_There are **1** instances of this issue:_

```solidity
File: /contracts/Exchange.sol

540: returns (uint256 price, uint256 tokenId, uint256 amount, AssetType assetType)
```

https://github.com/code-423n4/2022-11-non-fungible/blob/main/contracts/Exchange.sol
