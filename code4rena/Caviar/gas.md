# Caviar

## Gas Optimizations Report

### G-01 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y – 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **6** instances of this issue:_

```solidity
File: /src/Pair.sol

80: require(lpTokenAmount >= minLpTokenAmount, "Slippage: lp token amount out");

117: require(baseTokenOutputAmount >= minBaseTokenOutputAmount, "Slippage: base token amount out");

120: require(fractionalTokenOutputAmount >= minFractionalTokenOutputAmount, "Slippage: fractional token out");

157: require(inputAmount <= maxInputAmount, "Slippage: amount in");

189: require(outputAmount >= minOutputAmount, "Slippage: amount out");

367: require(block.timestamp >= closeTimestamp, "Not withdrawable yet");
```

https://github.com/code-423n4/2022-12-caviar/blob/main/src/Pair.sol

### G-02 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **3** instances of this issue:_

```solidity
File: /src/Pair.sol

448: balanceOf[from] -= amount;

453: balanceOf[to] += amount;
```

https://github.com/code-423n4/2022-12-caviar/blob/main/src/Pair.sol

```solidity
File: /src/lib/SafeERC20Namer.sol

35: charCount += uint8(b[i]);
```

https://github.com/code-423n4/2022-12-caviar/blob/main/src/lib/SafeERC20Namer.sol
