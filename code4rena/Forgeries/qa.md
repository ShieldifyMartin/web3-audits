# Forgeries

## QA Report

### L-01 `emit` function called early

_There are **1** instances of this issue:_

```solidity
File: /src/VRFNFTRandomDraw.sol

287: emit WinnerSentNFT(

312: emit OwnerReclaimedNFT(owner());
```

https://github.com/code-423n4/2022-12-forgeries/blob/main/src/VRFNFTRandomDraw.sol

### N-01 Non-floating `pragma` should be used

_There are **1** instances of this issue:_

```solidity
File: /src/utils/Version.sol
```
