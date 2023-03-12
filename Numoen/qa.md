# Numoen

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

`safeMint` does an unsafe external call to the recipient, so there is a reentrency attack vector. Make sure to keep 'nonReentrant' modifier.

```solidity
93: _mint(to, shares);
```

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/Lendgine.sol

### L-02 Check in `receive` function could be surrounded

```solidity
22: require(msg.sender == weth, "Not WETH9");
```

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/Payment.sol

### N-01 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

```solidity
16: require(y < 2 ** 255);
```

https://github.com/code-423n4/2023-01-numoen/blob/main/src/libraries/SafeCast.sol

### N-02 Too old solidity version is used

New features couldn't be used.

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/Factory.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/Lendgine.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/LiquidityManager.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/LendgineRouter.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/ImmutableState.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/JumpRate.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/Payment.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/SwapHelper.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/Pair.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/libraries/PositionMath.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/libraries/Balance.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/libraries/SafeCast.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/libraries/LendgineAddress.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/libraries/Position.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/UniswapV2/libraries/UniswapV2Library.sol

### N-03 Recommended functions order in solidity should be followed to improve code quality

It is recommended to stricly follow solidity best code-style practices.

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/Factory.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/Lendgine.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/LiquidityManager.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/LendgineRouter.sol

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/Payment.sol
