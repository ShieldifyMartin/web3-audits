# Size

## Gas Optimisation Report

### G-01 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **2** instances of this issue:_

```solidity
File: /SizeSealed.sol

294: data.filledBase += baseAmount;

373: b.baseWithdrawn += baseTokensAvailable;
```

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

### G-02 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas.

_There are **7** instances of this issue:_

```solidity
File: /SizeSealed.sol

35: } else if (block.timestamp <= a.timings.endTimestamp + 24 hours) {

60: if (timings.endTimestamp <= block.timestamp) {

63: if (timings.startTimestamp >= timings.endTimestamp) {

157: if (bidIndex >= 1000) {

270: if (quotePerBase >= data.previousQuotePerBase) {

425: if (block.timestamp >= a.timings.endTimestamp) {

426: if (a.data.lowestQuote != type(uint128).max || block.timestamp <= a.timings.endTimestamp + 24 hours) {
```

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

### G-03 Not using the named return variables when a `function` `returns` wastes deployment gas

_There are **2** instances of this issue:_

```solidity
File: /SizeSealed.sol

454: returns (uint128 tokensAvailable)
```

https://github.com/code-423n4/2022-11-size/blob/main/src/SizeSealed.sol

```solidity
File: /util/ECCMath.sol

54: returns (bytes32 decryptedMessage)
```

https://github.com/code-423n4/2022-11-size/blob/main/src/util/ECCMath.sol
