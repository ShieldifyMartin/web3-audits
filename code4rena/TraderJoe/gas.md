# Trader Joe

## Gas Optimisation Report

### G-01 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **36** instances of this issue:_

```solidity
File: /src/LBFactory.sol

143: baseFactor = _preset.decode(type(uint16).max, _shift += 16);

144: filterPeriod = _preset.decode(type(uint16).max, _shift += 16);

145: decayPeriod = _preset.decode(type(uint16).max, _shift += 16);

146: reductionFactor = _preset.decode(type(uint16).max, _shift += 16);

147: variableFeeControl = _preset.decode(type(uint24).max, _shift += 16);

148: protocolShare = _preset.decode(type(uint16).max, _shift += 24);

149: maxVolatilityAccumulated = _preset.decode(type(uint24).max, _shift += 16);
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBFactory.sol

```solidity
File: /src/LBPair.sol

227: cumulativeId += _activeId * _deltaT;

228: cumulativeVolatilityAccumulated += uint256(_fp.volatilityAccumulated) * _deltaT;

287: amountX += _amountX;

288: amountY += _amountY;

337: _amountOut += _amountOutOfBin;

452: _bins[_id].accTokenXPerShare += _feesX.getTokenPerShare(_totalSupply);

453: _bins[_id].accTokenYPerShare += _feesY.getTokenPerShare(_totalSupply);

508: (_mintInfo.totalDistributionX += _mintInfo.distributionX) > Constants.PRECISION ||

509: (_mintInfo.totalDistributionY += _mintInfo.distributionY) > Constants.PRECISION

540: _mintInfo.activeFeeX += _fees.total;

551: _mintInfo.activeFeeY += _fees.total;

572: _pair.reserveX += uint112(_mintInfo.amountX);

573: _pair.reserveY += uint112(_mintInfo.amountY);

575: _mintInfo.amountXAddedToPair += _mintInfo.amountX;

576: _mintInfo.amountYAddedToPair += _mintInfo.amountY;

639: amountY += _amountY;

646: amountX += _amountX;

711: amountX += _amountX;

712: amountY += _amountY;

896: (amountX += _amountX).safe128();

897: (amountY += _amountY).safe128();
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBPair.sol

```solidity
File: /src/LBRouter.sol

128: amountIn += _amountInWithFees;

129: feesIn += _fee;

130: _amountOut -= _amountOutOfBin;

173: _amountIn -= _amountInToBin + _fees.total;

174: feesIn += _fees.total;

175: amountOut += _amountOutOfBin;
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBRouter.sol

```solidity
File: /src/LBToken.sol

211: _totalSupplies[_id] += _amount;

241: _totalSupplies[_id] -= _amount;
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBToken.sol

### G-02 Using `!= 0` on uints costs less gas than `> 0`.

Up until Solidity 0.8.13

_There are **9** instances of this issue:_

```solidity
File: /src/LBFactory.sol

161: if (_nbPresets > 0) {

191: if (_nbAvailable > 0) {
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBFactory.sol

```solidity
File: /src/LBQuoter.sol

79: if (quote.pairs[i] != address(0) && quote.amounts[i] > 0) {

82: if (reserveIn > 0 && reserveOut > 0) {

99: if (LBPairsAvailable.length > 0 && quote.amounts[i] > 0) {

154: for (uint256 i = swapLength; i > 0; i--) {

157: if (quote.pairs[i - 1] != address(0) && quote.amounts[i] > 0) {

160: if (reserveIn > 0 && reserveOut > quote.amounts[i]) {

176: if (LBPairsAvailable.length > 0 && quote.amounts[i] > 0) {
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBQuoter.sol

### G-03 Functions that are access-restricted from most users may be marked as payable

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **16** instances of this issue:_

```solidity
File: /src/LBFactory.sol

215: function setLBPairImplementation(address _LBPairImplementation) external override onlyOwner {

317: ) external override onlyOwner {

350: ) external override onlyOwner {

396: function removePreset(uint16 _binStep) external override onlyOwner {

434: ) external override onlyOwner {

468: function setFeeRecipient(address _feeRecipient) external override onlyOwner {

474:  function setFlashLoanFee(uint256 _flashLoanFee) external override onlyOwner {

485: function setFactoryLockedState(bool _locked) external override onlyOwner {

493: function addQuoteAsset(IERC20 _quoteAsset) external override onlyOwner {

502: function removeQuoteAsset(IERC20 _quoteAsset) external override onlyOwner {

520: function forceDecay(ILBPair _LBPair) external override onlyOwner {
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBFactory.sol

```solidity
File: /src/LBPair.sol

110: ) external override onlyFactory {

788: function setFeesParameters(bytes32 _packedFeeParameters) external override onlyFactory {

792: function forceDecay() external override onlyFactory {
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBPair.sol

```solidity
File: /src/LBRouter.sol

626: ) external override onlyFactoryOwner {

647: ) external override onlyFactoryOwner {
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBRouter.sol

### G-04 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

_There are **5** instances of this issue:_

```solidity
File: /src/LBPair.sol

278: if (_lastId >= _id && i != 0) revert LBPair__OnlyStrictlyIncreasingId();

517: if (_mintInfo.id >= _pair.activeId) {

636: if (_id <= _activeId) {

643: if (_id >= _activeId) {

924: if (_oracleSize >= _newSize) revert LBPair__NewSizeTooSmall(_newSize, _oracleSize);
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBPair.sol

### G-05 The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

This change would save up to 6 gas per instance/loop.

_There are **4** instances of this issue:_

```solidity
File: /src/LBQuoter.sol

75: for (uint256 i; i < swapLength; i++) {

100: for (uint256 j; j < LBPairsAvailable.length; j++) {

177: for (uint256 j; j < LBPairsAvailable.length; j++) {
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBQuoter.sol

```solidity
File: /src/LBRouter.sol

711: for (uint256 i = _pairs.length; i != 0; i--) {
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBRouter.sol

### G-06 Not using the named `return` variables when a `function` `returns` wastes deployment gas

_There are **11** instances of this issue:_

```solidity
File: /src/libraries/Math512Bits.sol

34: ) internal pure returns (uint256 result) {

123: ) internal pure returns (uint256 result) {
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/libraries/Math512Bits.sol

```solidity
File: /src/libraries/Samples.sol

45: ) internal view returns (bytes32 packedSample) {

75: ) internal pure returns (bytes32 packedSample) {
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/libraries/Samples.sol

```solidity
File: /src/LBPair.sol

137: uint256 reserveX,

138: uint256 reserveY,

139: uint256 activeId

156: uint256 feesXTotal,

157: uint256 feesYTotal,

158: uint256 feesXProtocol,

159: uint256 feesYProtocol
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBPair.sol
