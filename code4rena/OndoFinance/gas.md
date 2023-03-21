# Ondo Finance

## Gas Optimizations Report

### G-01 Splitting `require()` statements that use `&&` saves gas

_There are **3** instances of this issue:_

Instead of using `&&` on single require check using two require checks can save gas

```solidity
292: require(
      (answeredInRound >= roundId) &&
        (updatedAt >= block.timestamp - maxChainlinkOracleTimeDelay),
      "Chainlink oracle price is stale"
    );
```

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/OndoPriceOracleV2.sol

```solidity
45: require(
      accrualBlockNumber == 0 && borrowIndex == 0,
      "market may only be initialized once"
    );
```

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cCash/CCash.sol

```solidity
45: require(
      accrualBlockNumber == 0 && borrowIndex == 0,
      "market may only be initialized once"
    );
```

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cToken/CTokenModified.sol

### G-02 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y – 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **3** instances of this issue:_

```solidity
92: require(block.timestamp <= deadline, "KYCRegistry: signature expired");
```

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/cash/kyc/KYCRegistry.sol

```solidity
417: borrowRateMantissa <= borrowRateMaxMantissa,
```

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cCash/CTokenCash.sol

```solidity
416: require(
      borrowRateMantissa <= borrowRateMaxMantissa,
      "borrow rate is absurdly high"
    );
```

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cToken/CTokenModified.sol
