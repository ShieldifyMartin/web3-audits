# Forgeries

## Gas Optimizations Report

### G-01 Use `constant` rather than `immutable` for state variables that do not change their value

_There are **6** instances of this issue:_

```solidity
File: /src/VRFNFTRandomDraw.sol

22: uint32 immutable callbackGasLimit = 200_000;

24: uint16 immutable minimumRequestConfirmations = 3;

26: uint16 immutable wordsRequested = 1;

29: uint256 immutable HOUR_IN_SECONDS = 60 * 60;

31: uint256 immutable WEEK_IN_SECONDS = (3600 * 24 * 7);

33: uint256 immutable MONTH_IN_SECONDS = (3600 * 24 * 7) * 30;
```

https://github.com/code-423n4/2022-12-forgeries/blob/main/src/VRFNFTRandomDraw.sol

### G-02 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **3** instances of this issue:_

```solidity
File: /src/VRFNFTRandomDraw.sol

173: function startDraw() external onlyOwner returns (uint256) {

203: function redraw() external onlyOwner returns (uint256) {

304: function lastResortTimelockOwnerClaimNFT() external onlyOwner {
```

https://github.com/code-423n4/2022-12-forgeries/blob/main/src/VRFNFTRandomDraw.sol

### G-03 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y – 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **1** instances of this issue:_

```solidity
File: /src/VRFNFTRandomDraw.sol#L315

204: if (request.drawTimelock >= block.timestamp) {
```

https://github.com/code-423n4/2022-12-forgeries/blob/main/src/VRFNFTRandomDraw.sol#L315
