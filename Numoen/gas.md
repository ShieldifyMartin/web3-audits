# Numoen

## Gas Optimizations Report

### G-01 Splitting `require()` statements that use `&&` saves gas

Instead of using `&&` on single require check using two require checks can save gas

```solidity
61: require(reserveIn > 0 && reserveOut > 0, "UniswapV2Library: INSUFFICIENT_LIQUIDITY");
```

https://github.com/code-423n4/2023-01-numoen/blob/main/src/periphery/UniswapV2/libraries/UniswapV2Library.sol

### G-02 Not using the named `return` variables anywhere in the `function` is confusing

Consider changing the variable to be an unnamed one

```solidity
13: function getBorrowRate(uint256 borrowedLiquidity, uint256 totalLiquidity) public pure override returns (uint256 rate) {

25: function getSupplyRate(

40: function utilizationRate(uint256 borrowedLiquidity, uint256 totalLiquidity) private pure returns (uint256 rate) {
```

https://github.com/code-423n4/2023-01-numoen/blob/main/src/core/JumpRate.sol
