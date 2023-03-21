# 3xcalibur

## QA Report

### L-01 Return values of `approve()` not checked

Not all IERC20 implementations revert() when there’s a failure in approve(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything.

_There are **3** instances of this issue:_

```solidity
File: /contracts/periphery/Multiswap.sol

57: IERC20(_token).approve(router, _amount);
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Multiswap.sol

```solidity
File: /contracts/periphery/Minter.sol

141: _token.approve(address(_voter), weekly);
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Minter.sol

```solidity
File: /contracts/periphery/Voter.sol

254: IERC20(base).approve(_gauge, type(uint).max);
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Voter.sol

### L-02 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **1** instances of this issue:_

```solidity
File: /contracts/periphery/Minter.sol

133: _token.mint(address(this), _required-_balanceOf);
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Minter.sol

### L-03 Modifiers shouldn’t update state

_There are **8** instances of this issue:_

```solidity
File: /contracts/periphery/Voter.sol

89: _unlocked = 2;

91: _unlocked = 1;
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Voter.sol

```solidity
File: /contracts/periphery/Bribe.sol

80: _unlocked = 2;

82: _unlocked = 1;
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Bribe.sol

```solidity
File: /contracts/Core/SwapPair.sol

115: _unlocked = 2;

117: _unlocked = 1;
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/Core/SwapPair.sol

```solidity
File: /contracts/periphery/Gauge.sol

102: _unlocked = 2;

104: _unlocked = 1;
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Gauge.sol

### N-01 `require()`/`revert()` statements should have descriptive reason strings

_There are **56** instances of this issue:_

```solidity
File: /contracts/periphery/Multiswap.sol

37: require(length > 1 && length <= 5 && _weights.length == length, "length");
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Multiswap.sol

```solidity
File: /contracts/Core/SwapFactory.sol

48: require(msg.sender == pauser);

53: require(msg.sender == pendingPauser);

58: require(msg.sender == pauser);

81: require(tokenA != tokenB, 'IA');

83: require(token0 != address(0), 'ZA');

84: require(getPair[token0][token1][stable] == address(0), 'PE');
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/Core/SwapFactory.sol

```solidity
File: /contracts/periphery/Minter.sol

35: require(msg.sender == admin);

71: require(initializer == msg.sender);
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Minter.sol

```solidity
File: /contracts/periphery/Voter.sol

88: require(_unlocked == 1);

98: require(msg.sender == minter);

140: require(IVotingEscrow(_ve).isApprovedOrOwner(msg.sender, _tokenId));

202: require(votes[_tokenId][_gauge] == 0);

203: require(_gaugeWeight != 0);

229: require(IVotingEscrow(_ve).isApprovedOrOwner(msg.sender, tokenId));

230: require(_gaugeVote.length == _weights.length);

240: require(!isWhitelisted[_token]);

272: require(isGauge[msg.sender]);

278:  require(isGauge[msg.sender]);

283: require(isGauge[msg.sender]);

289: require(isGauge[msg.sender]);

362:  require(IVotingEscrow(_ve).isApprovedOrOwner(msg.sender, _tokenId));

373: require(IVotingEscrow(_ve).isApprovedOrOwner(msg.sender, _tokenId));

434: require(token.code.length > 0);

437: require(success && (data.length == 0 || abi.decode(data, (bool))));
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Voter.sol

```solidity
File: /contracts/periphery/Bribe.sol

79: require(_unlocked == 1);

234: require(IVotingEscrow(_ve).isApprovedOrOwner(msg.sender, tokenId));

249: require(msg.sender == voter);

391: require(msg.sender == voter);

404: require(msg.sender == voter);

424: require(amount > 0);

441: require(amount > _left);

445: require(rewardRate[token] > 0);

454: require(token.code.length > 0);

457: require(success && (data.length == 0 || abi.decode(data, (bool))));

461: require(token.code.length > 0);

464: require(success && (data.length == 0 || abi.decode(data, (bool))));
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Bribe.sol

```solidity
File: /contracts/Core/SwapPair.sol

346: require(!SwapFactory(factory).isPaused());

372: require(_k(_balance0, _balance1) >= _k(_reserve0, _reserve1), 'K'); // SwapPair: K

538: require(token.code.length > 0);

541: require(success && (data.length == 0 || abi.decode(data, (bool))));
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/Core/SwapPair.sol

```solidity
File: /contracts/periphery/Gauge.sol

294: require(msg.sender == account || msg.sender == voter);

458: require(amount > 0);

466: require(IVotingEscrow(_ve).ownerOf(tokenId) == msg.sender);

471: require(tokenIds[msg.sender] == tokenId);

509: require(tokenId == tokenIds[msg.sender]);

536: require(token != stake);

537: require(amount > 0);

554: require(amount > _left);

558: require(rewardRate[token] > 0);

567: require(token.code.length > 0);

570: require(success && (data.length == 0 || abi.decode(data, (bool))));

574: require(token.code.length > 0);

577: require(success && (data.length == 0 || abi.decode(data, (bool))));

581: require(token.code.length > 0);

584: require(success && (data.length == 0 || abi.decode(data, (bool))));
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Gauge.sol

### N-02 `constants` should be defined rather than using magic numbers

_There are **8** instances of this issue:_

```solidity
File: /contracts/periphery/Multiswap.sol
46: uint amount_ = msg.value * _weights[i] / 10000;
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Multiswap.sol

```solidity
File: /contracts/Core/SwapFactory.sol

37: fee[true] = 369; // 0.0369% for stable swaps (hundredth of a basis point / 369/1000000)

38: fee[false] = 2700; // 0.27% for vaiable swaps (hundredth of a basis point / 2700/1000000)
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/Core/SwapFactory.sol

```solidity
File: /contracts/periphery/Minter.sol

59: active_period = (block.timestamp + (2*week)) / week * week;

84: require(block.timestamp >= last_epoch + 26 weeks, "must wait next period");

87: last_epoch = block.timestamp + 26 weeks;
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Minter.sol

```solidity
File: /contracts/periphery/Voter.sol

108: return (IERC20(base).totalSupply() - IERC20(_ve).totalSupply()) / 10000;
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Voter.sol

```solidity
File: /contracts/Core/SwapPair.sol

479: require(v == 27 || v == 28, 'SwapPair: INVALID_SIGNATURE');
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/Core/SwapPair.sol

### N-03 Not `using` the named `return` variables anywhere in the `function` is confusing

_There are **5** instances of this issue:_

```solidity
File: /contracts/Core/SwapFactory.sol

76: function getInitializable() external view returns (address, address, bool, uint) {
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/Core/SwapFactory.sol

```solidity
File: /contracts/periphery/Voter.sol

247: function createSwapGauge(address _pair) external returns (address gauge) {
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Voter.sol

```solidity
File: /contracts/Core/SwapPair.sol

128: function metadata() external view returns (uint dec0, uint dec1, uint r0, uint r1, bool st, address t0, address t1) {

265: function quote(address tokenIn, uint amountIn, uint granularity) external view returns (uint amountOut) {
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/Core/SwapPair.sol

```solidity
File: /contracts/periphery/Gauge.sol

110: function claimFees() external lock returns (uint claimed0, uint claimed1) {
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Gauge.sol

### N-04 Event is missing `indexed` fields

Each `event` should use three `indexed` fields if there are three or more fields

_There are **2** instances of this issue:_

```solidity
File: /contracts/periphery/Voter.sol

52: event Abstained(uint tokenId, uint256 weight);
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/periphery/Voter.sol

```solidity
File: /contracts/Core/SwapPair.sol

86: event Sync(uint reserve0, uint reserve1);
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/Core/SwapPair.sol

### N-05 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

_There are **1** instances of this issue:_

```solidity
File: /contracts/Core/SwapPair.sol

32: uint internal constant MINIMUM_LIQUIDITY = 10**3;
```

https://github.com/code-423n4/2022-10-3xcalibur/blob/main/contracts/Core/SwapPair.sol
