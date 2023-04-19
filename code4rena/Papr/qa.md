# Papr

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **2** instances of this issue:_

```solidity
File: /src/PaprToken.sol

25: _mint(to, amount);
```

https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/PaprToken.sol

```solidity
File: /src/PaprController.sol

476: PaprToken(address(papr)).mint(mintTo, amount);
```

https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/PaprController.sol

### N-01 Non-floating `pragma` should be used

_There are **5** instances of this issue:_

```solidity
File: /src/PaprToken.sol

File: /src/ReservoirOracleUnderwriter.sol

File: /src/UniswapOracleFundingRateController.sol

File: /src/UniswapOracleFundingRateController.sol

File: /src/PaprController.sol
```

### N-02 Should be splitted into two separate lines

_There are **1** instances of this issue:_

```solidity
File: /src/UniswapOracleFundingRateController.sol

112: revert PoolTokensDoNotMatch();
```

https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/UniswapOracleFundingRateController.sol

### N-03 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

_There are **2** instances of this issue:_

```solidity
File: /src/PaprController.sol

87: initSqrtRatio = UniswapHelpers.oneToOneSqrtRatio(underlyingONE, 10 ** 18);

89: initSqrtRatio = UniswapHelpers.oneToOneSqrtRatio(10 ** 18, underlyingONE);
```

https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/PaprController.sol
