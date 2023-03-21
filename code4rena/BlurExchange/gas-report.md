# Blur Exchange

## Gas Optimisation Report

### G-01 Use custom errors rather than `revert()`/`require()` strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **26** instances of this issue:_

```solidity
File: /lib/ReentrancyGuarded.sol

14: require(!reentrancyLock, "Reentrancy detected");
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/lib/ReentrancyGuarded.sol

```solidity
File: /PolicyManager.sol

26: require(!_whitelistedPolicies.contains(policy), "Already whitelisted");

37: require(_whitelistedPolicies.contains(policy), "Not whitelisted");
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/PolicyManager.sol

```solidity
File: /ExecutionDelegate.sol

22: require(contracts[msg.sender], "Contract is not approved to make transfers");

77: require(revokedApproval[from] == false, "User has revoked approval");

92: require(revokedApproval[from] == false, "User has revoked approval");

108: require(revokedApproval[from] == false, "User has revoked approval");

124: require(revokedApproval[from] == false, "User has revoked approval");
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/ExecutionDelegate.sol

```solidity
File: /BlurExchange.sol

36: require(isOpen == 1, "Closed");

134: require(sell.order.side == Side.Sell);

139: require(_validateOrderParameters(sell.order, sellHash), "Sell has invalid parameters");

140: require(_validateOrderParameters(buy.order, buyHash), "Buy has invalid parameters");

142: require(_validateSignatures(sell, sellHash), "Sell failed authorization");

143: require(_validateSignatures(buy, buyHash), "Buy failed authorization");

183: require(msg.sender == order.trader);

219: require(address(_executionDelegate) != address(0), "Address cannot be zero");

228: require(address(_policyManager) != address(0), "Address cannot be zero");

237: require(_oracle != address(0), "Address cannot be zero");

318: require(block.number - order.blockNumber < blockRange, "Signed block number out of range");

407: require(v == 27 || v == 28, "Invalid v parameter");

424: require(policyManager.isPolicyWhitelisted(sell.matchingPolicy), "Policy is not whitelisted");

428: require(policyManager.isPolicyWhitelisted(buy.matchingPolicy), "Policy is not whitelisted");

431: require(canMatch, "Orders cannot be matched");

452: require(msg.value == price);

482: require(totalFee <= price, "Total amount of fees are more than the price");

534: require(_exists(collection), "Collection does not exist");
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol

### G-02 The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

This change would save up to 6 gas per instance/loop.

_There are **4** instances of this issue:_

```solidity
File: /PolicyManager.sol

77: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/PolicyManager.sol

```solidity
File: /lib/MerkleVerifier.sol

38: for (uint256 i = 0; i < proof.length; i++) {
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/lib/MerkleVerifier.sol

```solidity
File: /BlurExchange.sol

199: for (uint8 i = 0; i < orders.length; i++) {

476: for (uint8 i = 0; i < fees.length; i++) {
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol

### G-03 It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied

Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

_There are **4** instances of this issue:_

```solidity
File: /PolicyManager.sol

77: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/PolicyManager.sol

```solidity
File: /lib/MerkleVerifier.sol

38: for (uint256 i = 0; i < proof.length; i++) {
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/lib/MerkleVerifier.sol

```solidity
File: /BlurExchange.sol

199: for (uint8 i = 0; i < orders.length; i++) {

476: for (uint8 i = 0; i < fees.length; i++) {
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol

### G-04 Not using the named `return` variables when a `function` returns wastes deployment gas

_There are **4** instances of this issue:_

```solidity
File: /lib/ERC1967Proxy.sol

29: function _implementation() internal view virtual override returns (address impl) {
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/lib/ERC1967Proxy.sol

```solidity
File: /lib/EIP712.sol

115: returns (bytes32 hash)

127: returns (bytes32 hash)

142: returns (bytes32 hash)
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/lib/EIP712.sol

### G-05 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **2** instances of this issue:_

```solidity
File: /BlurExchange.sol

208: nonces[msg.sender] += 1;

479: totalFee += fee;
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol

### G-06 `public` functions not called by the contract should be declared `external` instead

_There are **1** instances of this issue:_

```solidity
File: /BlurExchange.sol

95: function initialize(
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol

### G-07 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **3** instances of this issue:_

```solidity
File: /BlurExchange.sol

168: sell.order.listingTime <= buy.order.listingTime ? sell.order.trader : buy.order.trader,

422: if (sell.listingTime <= buy.listingTime) {

482: require(totalFee <= price, "Total amount of fees are more than the price");
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol

### G-08 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **6** instances of this issue:_

```solidity
File: /BlurExchange.sol

43: function open() external onlyOwner {

47: function close() external onlyOwner {

215: function setExecutionDelegate(IExecutionDelegate _executionDelegate)

224: function setPolicyManager(IPolicyManager _policyManager)

233: function setOracle(address _oracle)

242: function setBlockRange(uint256 _blockRange)
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol
