# M-01 Return values of `transfer()`/`transferFrom()` not checked

## Impact

`withdrawLPTokens#_transfer` calls IERC20 transfer and transferFrom directly. There are two issues with using this interface directly:

1. `withdrawLPTokens#_transfer` function does not check the return value of these calls. Tokens that return `false` rather than revert to indicate failed transfers may silently fail rather than reverting as expected.
2. Since the IERC20 interface requires a boolean return value, attempting to transfer ERC20s with missing return values will revert. This means `LSD Network` payment terminals cannot support a number of popular ERC20s, including USDT and BNB.

## Proof of Concept

```solidity
File: /contracts/liquid-staking/GiantPoolBase.sol

86: token.transfer(msg.sender, amount);
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantPoolBase.sol

```solidity
184: _lpTokens[i].transfer(address(this), _lpTokens[i].balanceOf(address(this)));
```

https://github.com/code-423n4/2022-11-stakehouse/blob/main/contracts/liquid-staking/GiantMevAndFeesPool.sol

## Recommended Migration Steps

`safeTransfer()` should be used rather than `transfer()`
