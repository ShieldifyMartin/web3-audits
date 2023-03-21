# Blur Exchange

## QA Report

### L-01 Modifiers shouldn’t update state

_There are **1** instances of this issue:_

```solidity
File: /lib/ReentrancyGuarded.sol

13: modifier reentrancyGuard {
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/lib/ReentrancyGuarded.sol

### L-02 `require()` should be used instead of `assert()`

_There are **1** instances of this issue:_

```solidity
File: /lib/ERC1967Proxy.sol

22: assert(_IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("eip1967.proxy.implementation")) - 1));
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/lib/ERC1967Proxy.sol

### L-03 Zero-`address` checks are missing

_There are **2** instances of this issue:_

```solidity
File: /PolicyManager.sol

25: function addPolicy(address policy) external override onlyOwner {

36: function removePolicy(address policy) external override onlyOwner {
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/PolicyManager.sol

### N-01 Use a more recent version of `solidity`

_There are **10** instances of this issue:_

```solidity
File: /lib/ReentrancyGuarded.sol

File: /lib/ERC1967Proxy.sol

File: /PolicyManager.sol

File: /matchingPolicies/StandardPolicyERC1155.sol

File: /matchingPolicies/StandardPolicyERC721.sol

File: /ExecutionDelegate.sol

File: /lib/EIP712.sol

File: /lib/MerkleVerifier.sol

File: /lib/OrderStructs.sol

File: /BlurExchange.sol
```

### N-02 Not using the named `return` variables anywhere in the `function` is confusing

_There are **1** instances of this issue:_

```solidity
File: /lib/ERC1967Proxy.sol

29: function _implementation() internal view virtual override returns (address impl) {
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/lib/ERC1967Proxy.sol

### N-03 `require()`/`revert()` statements should have descriptive reason strings

_There are **4** instances of this issue:_

```solidity
File: /lib/MerkleVerifier.sol

24: revert InvalidProof();
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/lib/MerkleVerifier.sol

```solidity
File: /BlurExchange.sol

134: require(sell.order.side == Side.Sell);

183: require(msg.sender == order.trader);

452: require(msg.value == price);
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol

### N-04 Event is missing indexed fields

_There are **8** instances of this issue:_

```solidity
File: /BlurExchange.sol

40: event Opened();

41: event Closed();

85: event OrderCancelled(bytes32 hash);

86: event NonceIncremented(address trader, uint256 newNonce);

88: event NewExecutionDelegate(IExecutionDelegate executionDelegate);

89: event NewPolicyManager(IPolicyManager policyManager);

90: event NewOracle(address oracle);

91: event NewBlockRange(uint256 blockRange);
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol

### N-05 `public` functions not called by the contract should be declared `external` instead

_There are **1** instances of this issue:_

```solidity
File: /BlurExchange.sol

95: function initialize(
```

https://github.com/code-423n4/2022-10-blur/blob/main/contracts/BlurExchange.sol
