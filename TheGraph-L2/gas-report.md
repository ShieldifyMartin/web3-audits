# The Graph L2

## Gas Optimisation Report

### G-01 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **12** instances of this issue:_

```solidity
File: /gateway/BridgeEscrow.sol

20: function initialize(address _controller) external onlyImpl {

28: function approveAll(address _spender) external onlyGovernor {

36: function revokeAll(address _spender) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/BridgeEscrow.sol

```solidity
File: /l2/token/L2GraphToken.sol

69: function setL1Address(address _addr) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol

```solidity
File: /upgrades/GraphProxyAdmin.sol

68: function changeProxyAdmin(IGraphProxy _proxy, address _newAdmin) public onlyGovernor {

77: function upgrade(IGraphProxy _proxy, address _implementation) public onlyGovernor {

86: function acceptProxy(GraphUpgradeable _implementation, IGraphProxy _proxy) public onlyGovernor {

100: ) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol

```solidity
File: /upgrades/GraphProxy.sol

104: function setAdmin(address _newAdmin) external ifAdmin {

115: function upgradeTo(address _newImplementation) external ifAdmin {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol

```solidity
File: /gateway/GraphTokenGateway.sol

30: function setPauseGuardian(address _newPauseGuardian) external onlyGovernor {

47: function setPaused(bool _newPaused) external onlyGovernorOrGuardian {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol

### G-02 Use custom errors rather than `revert()`/`require()` strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **64** instances of this issue:_

```solidity
File: /upgrades/GraphUpgradeable.sol

24: require(msg.sender == _proxy.admin(), "Caller must be the proxy admin");

32: require(msg.sender == _implementation(), "Caller must be the implementation");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphUpgradeable.sol

```solidity
File: /governance/Governed.sol

24: require(msg.sender == governor, "Only Governor can call");

41: require(_newGovernor != address(0), "Governor must be set");

54: require(
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol

```solidity
File: /l2/token/L2GraphToken.sol

36: require(msg.sender == gateway, "NOT_GATEWAY");

49: require(_owner != address(0), "Owner must be set");

60: require(_gw != address(0), "INVALID_GATEWAY");

70: require(_addr != address(0), "INVALID_L1_ADDRESS");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol

```solidity
File: /upgrades/GraphProxyAdmin.sol

34: require(success);

47: require(success);

59: require(success);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol

```solidity
File: /upgrades/GraphProxyStorage.sol

62: require(msg.sender == _admin(), "Caller must be admin");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyStorage.sol

```solidity
File: /upgrades/GraphProxy.sol

105: require(_newAdmin != address(0), "Cannot change the admin of a proxy to the zero address");

133: require(success);

141: require(Address.isContract(_pendingImplementation), "Implementation must be a contract");

142: require(

157: require(msg.sender != _admin(), "Cannot fallback to proxy target");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxy.sol

```solidity
File: /governance/Managed.sol

44: require(!controller.paused(), "Paused");

45: require(!controller.partialPaused(), "Partial-paused");

49: require(!controller.paused(), "Paused");

53: require(msg.sender == controller.getGovernor(), "Caller must be Controller governor");

57: require(msg.sender == address(controller), "Caller must be Controller");

104: require(_controller != address(0), "Controller must be set");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol

```solidity
File: /l2/token/GraphTokenUpgradeable.sol

60: require(isMinter(msg.sender), "Only minter can call");

94: require(_owner == recoveredAddress, "GRT: invalid permit");

95: require(_deadline == 0 || block.timestamp <= _deadline, "GRT: expired permit");

106: require(_account != address(0), "INVALID_MINTER");

115: require(_minters[_account], "NOT_A_MINTER");

123: require(_minters[msg.sender], "NOT_A_MINTER");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

```solidity
File: /l2/gateway/L2GraphTokenGateway.sol

69: require(

98: require(_l2Router != address(0), "INVALID_L2_ROUTER");

108: require(_l1GRT != address(0), "INVALID_L1_GRT");

118: require(_l1Counterpart != address(0), "INVALID_L1_COUNTERPART");

145: require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

146: require(_amount > 0, "INVALID_ZERO_AMOUNT");

147: require(msg.value == 0, "INVALID_NONZERO_VALUE");

148: require(_to != address(0), "INVALID_DESTINATION");

153: require(outboundCalldata.extraData.length == 0, "CALL_HOOK_DATA_NOT_ALLOWED");

233: require(_l1Token == l1GRT, "TOKEN_NOT_GRT");

234: require(msg.value == 0, "INVALID_NONZERO_VALUE");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol

```solidity
File: /gateway/L1GraphTokenGateway.sol

74: require(inbox != address(0), "INBOX_NOT_SET");

78: require(msg.sender == address(bridge), "NOT_FROM_BRIDGE");

82: require(l2ToL1Sender == l2Counterpart, "ONLY_COUNTERPART_GATEWAY");

110: require(_inbox != address(0), "INVALID_INBOX");

111: require(_l1Router != address(0), "INVALID_L1_ROUTER");

122: require(_l2GRT != address(0), "INVALID_L2_GRT");

132: require(_l2Counterpart != address(0), "INVALID_L2_COUNTERPART");

142: require(_escrow != address(0) && Address.isContract(_escrow), "INVALID_ESCROW");

153: require(_newWhitelisted != address(0), "INVALID_ADDRESS");

154: require(!callhookWhitelist[_newWhitelisted], "ALREADY_WHITELISTED");

165: require(_notWhitelisted != address(0), "INVALID_ADDRESS");

166: require(callhookWhitelist[_notWhitelisted], "NOT_WHITELISTED");

200: require(_l1Token == address(token), "TOKEN_NOT_GRT");

201: require(_amount > 0, "INVALID_ZERO_AMOUNT");

202: require(_to != address(0), "INVALID_DESTINATION");

213: require(

217: require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");

224: require(msg.value >= expectedEth, "WRONG_ETH_VALUE");

271: require(_l1Token == address(token), "TOKEN_NOT_GRT");

275: require(_amount <= escrowBalance, "BRIDGE_OUT_OF_FUNDS");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

```solidity
File: /gateway/GraphTokenGateway.sol

19: require(

31: require(_newPauseGuardian != address(0), "PauseGuardian must be set");

40: require(!_paused, "Paused (contract)");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/GraphTokenGateway.sol

### G-03 Splitting `require()` statements that use `&&` saves gas

Instead of using && on single require check using two require checks can save gas

_There are **1** instances of this issue:_

```solidity
File: /governance/Governed.sol

55: pendingGovernor != address(0) && msg.sender == pendingGovernor,
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol

### G-04 Expressions for `constant` values such as a call to `keccak256()`, should use `immutable` rather than `constant`

It is expected that the value should be converted into a constant value at compile time. But actually the expression is re-calculated each time the constant is referenced.

_There are **4** instances of this issue:_

```solidity
File: /l2/token/GraphTokenUpgradeable.sol

34: bytes32 private constant DOMAIN_TYPE_HASH =

38: bytes32 private constant DOMAIN_NAME_HASH = keccak256("Graph Token");

39: bytes32 private constant DOMAIN_VERSION_HASH = keccak256("0");

42: bytes32 private constant PERMIT_TYPEHASH =
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

### G-05 Using `!= 0` on uints costs less gas than `> 0`

_There are **3** instances of this issue:_

```solidity
File: /l2/gateway/L2GraphTokenGateway.sol

238: if (_data.length > 0) {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol

```solidity
File: /gateway/L1GraphTokenGateway.sol

201: require(_amount > 0, "INVALID_ZERO_AMOUNT");

217: require(maxSubmissionCost > 0, "NO_SUBMISSION_COST");
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

### G-06 Don't compare boolean expressions to boolean literals

Use if(x) / if(!x) instead of if(x == true) / if(x == false).

_There are **1** instances of this issue:_

```solidity
File: /gateway/L1GraphTokenGateway.sol

214: extraData.length == 0 || callhookWhitelist[msg.sender] == true,
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol
