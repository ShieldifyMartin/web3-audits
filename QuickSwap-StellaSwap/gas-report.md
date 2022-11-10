# QuickSwap and StellaSwap

## Gas Optimisation Report

### G-01 Use custom errors rather than `revert()`/`require()` strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **57** instances of this issue:_

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

45: require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');

46: require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

238: require(lteConsideringOverflow(self[oldestIndex].blockTimestamp, target, time), 'OLD');

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
File: src/core/contracts/libraries/TickTable.sol

15: require(tick % Constants.TICK_SPACING == 0, 'tick is not spaced');
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol

```solidity
File: src/core/contracts/libraries/TokenDeltaMath.sol

30: require(priceDelta < priceUpper); // forbids underflow and 0 priceLower

51: require(priceUpper >= priceLower);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TokenDeltaMath.sol

```solidity
File: src/core/contracts/base/PoolState.sol

41: require(globalState.unlocked, 'LOK');
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/base/PoolState.sol

### G-02 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **22** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraFactory.sol

109: require(uint256(alpha1) + uint256(alpha2) + uint256(baseFee) <= type(uint16).max, 'Max fee exceeded');
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraFactory.sol

```solidity
File: src/core/contracts/AlgebraPool.sol

229: require((_blockTimestamp() - lastLiquidityAddTimestamp) >= _liquidityCooldown);

474: require((amount0 = uint256(amount0Int)) <= receivedAmount0, 'IIAM2');

475: require((amount1 = uint256(amount1Int)) <= receivedAmount1, 'IIAM2');

608: require(balance0Before.add(uint256(amount0)) <= balanceToken0(), 'IIA');

614: require(balance1Before.add(uint256(amount1)) <= balanceToken1(), 'IIA');

921: require(balance0Before.add(fee0) <= paid0, 'F0');

935: require(balance1Before.add(fee1) <= paid1, 'F1');

953: require((communityFee0 <= Constants.MAX_COMMUNITY_FEE) && (communityFee1 <= Constants.MAX_COMMUNITY_FEE));

968: require(newLiquidityCooldown <= Constants.MAX_LIQUIDITY_COOLDOWN && liquidityCooldown != newLiquidityCooldown);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

52: if (x >= 6 * uint256(g)) return alpha; // so x < 19 bits

59: if (x >= 6 * uint256(g)) return 0; // so x < 19 bits
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

100: if (res == b > currentTime) res = a <= b;

156: uint256 right = lastIndex >= oldestIndex ? lastIndex : lastIndex + UINT16_MODULO; // newest timepoint considering one index overflow
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol

```solidity
File: src/core/contracts/libraries/PriceMovementMath.sol

64: if (denominator >= liquidityShifted) return uint160(FullMath.mulDivRoundingUp(liquidityShifted, price, denominator)); // always fits in 160 bits

80: .add(amount <= type(uint160).max ? (amount << Constants.RESOLUTION) / liquidity : FullMath.mulDiv(amount, Constants.Q96, liquidity))

83: uint256 quotient = amount <= type(uint160).max

155: if (amountAvailable >= 0) {

159: if (amountAvailableAfterFee >= input) {

180: if (uint256(amountAvailable) >= output) resultPrice = targetPrice;
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/PriceMovementMath.sol

```solidity
File: src/core/contracts/libraries/TickManager.sol

51: if (currentTick >= bottomTick) {

109: if (tick <= currentTick) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol

### G-03 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **37** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraPool.sol

257: _position.fees0 += fees0;

258: _position.fees1 += fees1;

804: amountRequired += step.output.toInt256(); // increase remaining output amount (since its negative)

801: amountRequired -= (step.input + step.feeAmount).toInt256(); // decrease remaining input amount

810: step.feeAmount -= delta;

811: communityFeeAmount += delta;

814: if (currentLiquidity > 0) cache.totalFeeGrowth += FullMath.mulDiv(step.feeAmount, Constants.Q128, currentLiquidity);

922: paid0 -= balance0Before;

931: totalFeeGrowth0Token += FullMath.mulDiv(paid0 - fees0, Constants.Q128, _liquidity);

936: paid1 -= balance1Before;

945: totalFeeGrowth1Token += FullMath.mulDiv(paid1 - fees1, Constants.Q128, _liquidity);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol

```solidity
File: src/core/contracts/DataStorageOperator.sol

45: require(uint256(_feeConfig.alpha1) + uint256(_feeConfig.alpha2) + uint256(_feeConfig.baseFee) <= type(uint16).max, 'Max fee exceeded');

138: if (volume >= 2**192) volumeShifted = (type(uint256).max) / (liquidity > 0 ? liquidity : 1);

140: if (volumeShifted >= MAX_VOLUME_PER_LIQUIDITY) return MAX_VOLUME_PER_LIQUIDITY;
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

84: res += xLowestDegree * gHighestDegree;

88: res += (xLowestDegree * gHighestDegree) / 2;

92: res += (xLowestDegree * gHighestDegree) / 6;

96: res += (xLowestDegree * gHighestDegree) / 24;

100: res += (xLowestDegree * gHighestDegree) / 120;

104: res += (xLowestDegree * gHighestDegree) / 720;

107: res += (xLowestDegree * g) / 5040 + (xLowestDegree * x) / (40320);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol

```solidity
File: src/core/contracts/libraries/DataStorage.sol

79: last.tickCumulative += int56(tick) * delta;

80: last.secondsPerLiquidityCumulative += ((uint160(delta) << 128) / (liquidity > 0 ? liquidity : 1)); // just timedelta if liquidity == 0

81: last.volatilityCumulative += uint88(_volatilityOnRange(delta, prevTick, tick, last.averageTick, averageTick)); // always fits 88 bits

83: last.volumePerLiquidityCumulative += volumePerLiquidity;

119: index -= 1; // considering underflow

251: beforeOrAt.tickCumulative += ((atOrAfter.tickCumulative - beforeOrAt.tickCumulative) / timepointTimeDelta) * targetDelta;

252: beforeOrAt.secondsPerLiquidityCumulative += uint160(

255: beforeOrAt.volatilityCumulative += ((atOrAfter.volatilityCumulative - beforeOrAt.volatilityCumulative) / timepointTimeDelta) * targetDelta;

256: beforeOrAt.volumePerLiquidityCumulative +=
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol

```solidity
File: src/core/contracts/libraries/TickManager.sol

58: innerFeeGrowth0Token -= upper.outerFeeGrowth0Token;

59: innerFeeGrowth1Token -= upper.outerFeeGrowth1Token;
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickManager.sol

```solidity
File: src/core/contracts/libraries/TickTable.sol

92: tick -= int24(255 - getMostSignificantBit(_row));

95: tick -= int24(bitNumber);

100: tick += 1;

112: tick += int24(getSingleSignificantBit(-_row & _row)); // least significant bit

115: tick += int24(255 - bitNumber);
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/TickTable.sol

### G-04 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **2** instances of this issue:_

```solidity
File: src/core/contracts/AlgebraPool.sol

952: function setCommunityFee(uint8 communityFee0, uint8 communityFee1) external override lock onlyFactoryOwner {

967: function setLiquidityCooldown(uint32 newLiquidityCooldown) external override onlyFactoryOwner {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/AlgebraPool.sol

### G-05 Splitting `require()` statements that use && saves gas

Instead of using && on single require check using two require checks can save gas

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/DataStorageOperator.sol

46: require(_feeConfig.gamma1 != 0 && _feeConfig.gamma2 != 0 && _feeConfig.volumeGamma != 0, 'Gammas must be > 0');
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol

### G-06 Use calldata instead of memory for function parameters

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **2** instances of this issue:_

```solidity
File: src/core/contracts/DataStorageOperator.sol

90: uint32[] memory secondsAgos,
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/DataStorageOperator.sol

```solidity
File: src/core/contracts/libraries/AdaptiveFee.sol

28: Configuration memory config
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/AdaptiveFee.sol

### G-07 The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

This change would save up to 6 gas per instance/loop.

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/libraries/DataStorage.sol

307: for (uint256 i = 0; i < secondsAgos.length; i++) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol

### G-08 It costs more gas to initialize non-constant/non-immutable variables to zero than to let the default of zero be applied

Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

_There are **1** instances of this issue:_

```solidity
File: src/core/contracts/libraries/DataStorage.sol#L87

307: for (uint256 i = 0; i < secondsAgos.length; i++) {
```

https://github.com/code-423n4/2022-09-quickswap/blob/main/src/core/contracts/libraries/DataStorage.sol#L87

### G-09 Not using the named return variables when a function returns wastes deployment gas

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
