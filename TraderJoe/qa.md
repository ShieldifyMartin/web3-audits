# Trader Joe

## QA Report

### L-01 Modifiers shouldn’t update state

_There are **2** instances of this issue:_

```solidity
File: /src/libraries/ReentrancyGuardUpgradeable.sol

47: _status = _ENTERED;

53:  _status = _NOT_ENTERED;
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/libraries/ReentrancyGuardUpgradeable.sol

### L-02 `require()` should be used instead of `assert()`

_There are **1** instances of this issue:_

```solidity
File: /src/LBFactory.sol

141: assert(_binStep == _preset.decode(type(uint16).max, _shift));
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBFactory.sol

### L-03 Zero-address checks are missing

_There are **1** instances of this issue:_

```solidity
File: /src/LBFactory.sol

469: _setFeeRecipient(_feeRecipient);
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBFactory.sol

### L-04 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **1** instances of this issue:_

```solidity
File: /src/LBPair.sol

579: _mint(_to, _mintInfo.id, _liquidity);
```

https://github.com/code-423n4/2022-10-traderjoe/blob/main/src/LBPair.sol

### N-01 Not using the named `return` variables anywhere in the `function` is confusing

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

### N-02 Use a more recent version of `solidity`

_There are **6** instances of this issue:_

```solidity
File: /src/LBErrors.sol

File: /src/LBFactory.sol

File: /src/LBPair.sol

File: /src/LBQuoter.sol

File: /src/LBRouter.sol

File: /src/LBToken.sol
```
