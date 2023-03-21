# Looks Rare

## QA Report

### L-01 Unused/empty `receive()`/`fallback()` function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds

_There are **2** instances of this issue:_

```solidity
File: /contracts/proxies/SeaportProxy.sol

86: receive() external payable {}
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/proxies/SeaportProxy.sol

```solidity
File: /contracts/LooksRareAggregator.sol

220: receive() external payable {}
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/LooksRareAggregator.sol

### L-02 Modifiers shouldn’t update state

_There are **2** instances of this issue:_

```solidity
File: /contracts/ReentrancyGuard.sol

24: _status = 2;

26: _status = 1;
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/ReentrancyGuard.sol

### L-03 No reverts even when `execute` delegatecall fails

_There are **1** instances of this issue:_

```solidity
File: /contracts/LooksRareAggregator.sol

88: (bool success, bytes memory returnData) = singleTradeData.proxy.delegatecall(proxyCalldata);
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/LooksRareAggregator.sol

### N-01 Non-floating `pragma` should be used

_There are **11** instances of this issue:_

```solidity
File: /contracts/ReentrancyGuard.sol

File: /contracts/OwnableTwoSteps.sol

File: /contracts/SignatureChecker.sol

File: /contracts/interfaces/IERC20.sol

File: /contracts/interfaces/IERC721.sol

File: /contracts/interfaces/IERC1155.sol

File: /contracts/lowLevelCallers/LowLevelERC1155Transfer.sol

File: /contracts/lowLevelCallers/LowLevelERC20Approve.sol

File: /contracts/lowLevelCallers/LowLevelERC20Transfer.sol

File: /contracts/lowLevelCallers/LowLevelERC721Transfer.sol

File: /contracts/lowLevelCallers/LowLevelETH.sol
```

### N-02 Event is missing `indexed` fields

Each `event` should use three `indexed` fields if there are three or more fields

_There are **2** instances of this issue:_

```solidity
File: /contracts/interfaces/ILooksRareAggregator.sol

36: event ERC20EnabledLooksRareAggregatorSet();

44: event FeeUpdated(address proxy, uint256 bp, address recipient);
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/interfaces/ILooksRareAggregator.sol
