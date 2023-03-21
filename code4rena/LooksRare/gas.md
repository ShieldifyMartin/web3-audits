# Looks Rare

## Gas Optimizations Report

### G-01 State variables can be packed into fewer storage slots

_There are **5** instances of this issue:_

```solidity
File: /contracts/libraries/seaport/ConsiderationStructs.sol

22: struct OrderComponents {

84: struct ReceivedItem {

100: struct BasicOrderParameters {

139: struct OrderParameters {
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/libraries/seaport/ConsiderationStructs.sol

```solidity
File: /contracts/libraries/OrderStructs.sol

6: struct BasicOrder {
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/libraries/OrderStructs.sol

### G-02 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **3** instances of this issue:_

```solidity
File: /contracts/proxies/SeaportProxy.sol

109: if (orders[i].currency == address(0)) ethValue += orders[i].price;

150: fee += orderFee;

209: fee += orderFee;
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/proxies/SeaportProxy.sol

### G-03 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **13** instances of this issue:_

```solidity
File: /contracts/OwnableTwoSteps.sol

51: function cancelOwnershipTransfer() external onlyOwner {

68: function confirmOwnershipRenouncement() external onlyOwner {

98: function initiateOwnershipTransfer(address newPotentialOwner) external onlyOwner {

110: function initiateOwnershipRenouncement() external onlyOwner {
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/OwnableTwoSteps.sol

```solidity
File: /contracts/TokenRescuer.sol

22: function rescueETH(address to) external onlyOwner {

34: function rescueERC20(address currency, address to) external onlyOwner {
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/TokenRescuer.sol

```solidity
File: /contracts/LooksRareAggregator.sol

120: function setERC20EnabledLooksRareAggregator(address _erc20EnabledLooksRareAggregator) external onlyOwner {

132: function addFunction(address proxy, bytes4 selector) external onlyOwner {

143: function removeFunction(address proxy, bytes4 selector) external onlyOwner {

157: ) external onlyOwner {

175: ) external onlyOwner {

199: ) external onlyOwner {

216: ) external onlyOwner {
```

https://github.com/code-423n4/2022-11-looksrare/blob/main/contracts/LooksRareAggregator.sol
