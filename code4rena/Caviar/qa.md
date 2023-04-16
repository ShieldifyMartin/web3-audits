# Caviar

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **1** instances of this issue:_

```solidity
File: /src/Pair.sol

90: lpToken.mint(msg.sender, lpTokenAmount);
```

https://github.com/code-423n4/2022-12-caviar/blob/main/src/Pair.sol

### N-01 Non-floating `pragma` should be used

_There are **4** instances of this issue:_

```solidity
File: /src/Caviar.sol

File: /src/Pair.sol

File: /src/LpToken.sol

File: /src/lib/SafeERC20Namer.sol
```
