# QuickSwap and StellaSwap

## QA Report

### L-01 require() should be used instead of assert()

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/libraries/DataStorage.sol

190: assert(false);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol

### N-01 Use a more recent version of solidity

_There are **13** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

File: src/core/contracts/AlgebraPool.sol

File: src/core/contracts/AlgebraPoolDeployer.sol

File: src/core/contracts/DataStorageOperator.sol

File: src/core/contracts/libraries/AdaptiveFee.sol

File: src/core/contracts/libraries/Constants.sol

File: src/core/contracts/libraries/DataStorage.sol

File: src/core/contracts/libraries/PriceMovementMath.sol

File: src/core/contracts/libraries/TickManager.sol

File: src/core/contracts/libraries/TickTable.sol

File: src/core/contracts/libraries/TokenDeltaMath.sol

File: src/core/contracts/base/PoolImmutables.sol

File: src/core/contracts/base/PoolState.sol
```

### N-02 `require()` / `revert()` statements should have descriptive reason strings

_There are **52** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

43: require(msg.sender == owner);

60: require(tokenA != tokenB);

62: require(token0 != address(0));

63: require(poolByPair[token0][token1] == address(0));

78: require(owner != _owner);

85: require(farmingAddress != _farmingAddress);

92: require(vaultAddress != _vaultAddress);

109: require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');

110: require(gamma1 != 0 && gamma2 != 0 && volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol

```solidity
File: src/core/contracts/AlgebraPool.sol

55: require(msg.sender == IAlgebraFactory(factory).owner());

60: require(topTick < TickMath.MAX_TICK + 1, 'TUM');

61: require(topTick > bottomTick, 'TLU');

62: require(bottomTick > TickMath.MIN_TICK - 1, 'TLM');

122: require(_lower.initialized);

134: require(_upper.initialized);

194: require(globalState.price == 0, 'AI');

224: require(currentLiquidity > 0, 'NP'); // Do not recalculate the empty ranges

229: require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);

434: require(liquidityDesired > 0, 'IL');

454: if (amount0 > 0) require((receivedAmount0 = balanceToken0() - receivedAmount0) > 0, 'IIAM');

455: if (amount1 > 0) require((receivedAmount1 = balanceToken1() - receivedAmount1) > 0, 'IIAM');

469: require(liquidityActual > 0, 'IIL2');

474: require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');

475: require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');

608: require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');

614: require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');

636: require(globalState.unlocked, 'LOK');

641: require((amountRequired = int256(balanceToken0().sub(balance0Before))) > 0, 'IIA');

645: require((amountRequired = int256(balanceToken1().sub(balance1Before))) > 0, 'IIA');

731: require(unlocked, 'LOK');

733: require(amountRequired != 0, 'AS');

739: require(limitSqrtPrice < currentPrice && limitSqrtPrice > TickMath.MIN_SQRT_RATIO, 'SPL');

743: require(limitSqrtPrice > currentPrice && limitSqrtPrice < TickMath.MAX_SQRT_RATIO, 'SPL');

898: require(_liquidity > 0, 'L');

921: require(balance0Before.add(fee0) <= paid0, 'F0');

953: require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

960: require(msg.sender == IAlgebraFactory(factory).farmingAddress());

968: require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/AlgebraPoolDeployer.sol

22: require(msg.sender == factory);

27: require(msg.sender == owner);

37: require(_factory != address(0));

38: require(factory == address(0));
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPoolDeployer.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

43: require(msg.sender == factory || msg.sender == IAlgebraFactory(factory).owner());
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

369: require(!self[0].initialized);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

52: require(price > 0);

53: require(liquidity > 0);

70: require((product = amount * price) / amount == price); // if the product overflows, we know the denominator underflows

71: require(liquidityShifted > product); // in addition, we must check that the denominator does not underflow

87: require(price > quotient);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol

```solidity
File: src/core/contracts/libraries/TickManager.sol

96: require(liquidityTotalAfter < Constants.MAX_LIQUIDITY_PER_TICK + 1, 'LO');
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol

```solidity
File: src/core/contracts/libraries/TokenDeltaMath.sol

30: require(priceDelta < priceUpper); // forbids underflow and 0 priceLower

51: require(priceUpper >= priceLower);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol

### N-03 No use of two-phase ownership transfers

Consider adding a two-phase transfer, where the current owner nominates the next owner, and the next owner has to call accept\*() to become the new owner. This prevents passing the ownership to an account that is unable to use it.

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

77: function setOwner(address _owner) external override onlyOwner {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol

### N-04 Not using the named return variables anywhere in the function is confusing

_There are **13** instances of this issue:_

```solidity
File: src/core/contracts/DataStorageOperator.sol

115: ) external view override onlyPool returns (uint112 TWVolatilityAverage, uint256 TWVolumePerLiqAverage) {

126: ) external override onlyPool returns (uint16 indexUpdated) {

135: ) external pure override returns (uint128 volumePerLiquidity) {

155: ) external view override onlyPool returns (uint16 fee) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

29: ) internal pure returns (uint16 fee)
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

213: ) internal view returns (Timepoint memory targetTimepoint) {

332: ) internal view returns (uint88 volatilityAverage, uint256 volumePerLiqAverage) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

25: ) internal pure returns (uint160 resultPrice) {

41: ) internal pure returns (uint160 resultPrice) {

51: ) internal pure returns (uint160 resultPrice) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol

```solidity
File: src/core/contracts/libraries/TickManager.sol

137: ) internal returns (int128 liquidityDelta) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol

```solidity
File: src/core/contracts/libraries/TickTable.sol

46: function getMostSignificantBit(uint256 word) internal pure returns (uint8 mostBitPos) {

72: ) internal view returns (int24 nextTick, bool initialized) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol
