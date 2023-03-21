# The Graph L2

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **2** instances of this issue:_

```solidity
File: /l2/token/L2GraphToken.sol

81: _mint(_account, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol

```solidity
File: /l2/token/GraphTokenUpgradeable.sol

133: _mint(_to, _amount);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/GraphTokenUpgradeable.sol

### L-02 Zero-`address` checks are missing

_There are **3** instances of this issue:_

```solidity
File: /governance/Governed.sol

31: function _initialize(address _initGovernor) internal {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Governed.sol

```solidity
File: /l2/token/L2GraphToken.sol

80: function bridgeMint(address _account, uint256 _amount) external override onlyGateway {

90: function bridgeBurn(address _account, uint256 _amount) external override onlyGateway {
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol

### N-01 Use a more recent version of solidity

_There are **23** instances of this issue:_

```solidity
File: /gateway/BridgeEscrow.sol

File: /upgrades/GraphUpgradeable.sol

File: /governance/Governed.sol

File: /governance/Pausable.sol

File: /l2/token/L2GraphToken.sol

File: /upgrades/GraphProxyAdmin.sol

File: /upgrades/GraphProxyStorage.sol

File: /upgrades/GraphProxy.sol

File: /governance/Managed.sol

File: /l2/token/GraphTokenUpgradeable.sol

File: /l2/gateway/L2GraphTokenGateway.sol

File: /gateway/L1GraphTokenGateway.sol

File: /gateway/GraphTokenGateway.sol

File: /curation/IGraphCurationToken.sol

File: /gateway/ICallhookReceiver.sol

File: /upgrades/IGraphProxy.sol

File: /epochs/IEpochManager.sol

File: /governance/IController.sol

File: /token/IGraphToken.sol

File: /rewards/IRewardsManager.sol

File: /staking/IStakingData.sol

File: /curation/ICuration.sol

File: /staking/IStaking.sol
```

### N-02 `Event` is missing indexed fields

Each event should use three indexed fields if there are three or more fields

_There are **15** instances of this issue:_

```solidity
File: /governance/Pausable.sol

19: event PartialPauseChanged(bool isPaused);

20: event PauseChanged(bool isPaused);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Pausable.sol

```solidity
File: /l2/token/L2GraphToken.sol

28: event GatewaySet(address gateway);

30: event L1AddressSet(address l1Address);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/token/L2GraphToken.sol

```solidity
File: /governance/Managed.sol

33: event ParameterUpdated(string param);

34: event SetController(address controller);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/governance/Managed.sol

```solidity
File: /l2/gateway/L2GraphTokenGateway.sol

58: event L2RouterSet(address l2Router);

60: event L1TokenAddressSet(address l1GRT);

62: event L1CounterpartAddressSet(address l1Counterpart);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/l2/gateway/L2GraphTokenGateway.sol

```solidity
File: /gateway/L1GraphTokenGateway.sol

event ArbitrumAddressesSet(address inbox, address l1Router);

event L2TokenAddressSet(address l2GRT);

event L2CounterpartAddressSet(address l2Counterpart);

event EscrowAddressSet(address escrow);

event AddedToCallhookWhitelist(address newWhitelisted);

event RemovedFromCallhookWhitelist(address notWhitelisted);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/gateway/L1GraphTokenGateway.sol

### N-03 `require()`/`revert()` statements should have descriptive reason strings

_There are **3** instances of this issue:_

```solidity
File: /upgrades/GraphProxyAdmin.sol

34: require(success);

47: require(success);

59: require(success);
```

https://github.com/code-423n4/2022-10-thegraph/blob/main/contracts/upgrades/GraphProxyAdmin.sol
