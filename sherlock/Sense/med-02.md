# [M-02] Deprecated `safeApprove()`

## Summary

Using deprecated function which leads to front-running possibility and potential unintended reverts and locked funds.

## Vulnerability Detail

This behavior is not natively present when calling `approve()` and can therefore easily create unintended reverts that lock funds in smart contracts that use this code. The front-running vector is also present.

## Impact

These functions can be front-run.

## Code Snippet

```solidity
86: ERC20(_target).safeApprove(divider, type(uint256).max);

87: ERC20(_adapterParams.stake).safeApprove(divider, type(uint256).max);
```

https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/adapters/abstract/BaseAdapter.sol#L86
https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/adapters/abstract/BaseAdapter.sol#L87

```solidity
128: ERC20(stake).safeApprove(address(divider), stakeSize);

491: target.safeApprove(address(divider), type(uint256).max);

492: target.safeApprove(address(adapter), type(uint256).max);

493: ERC20(Adapter(adapter).underlying()).safeApprove(address(adapter), type(uint256).max);

509: ERC20(assetIn).safeApprove(address(balancerVault), amountIn);

861: liq.tokens[i].safeApprove(address(balancerVault), liq.amounts[i]);

908: ERC20(address(quote.sellToken)).safeApprove(quote.spender, type(uint256).max);
```

https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/Periphery.sol#L128
https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/Periphery.sol#L491
https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/Periphery.sol#L492
https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/Periphery.sol#L493
https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/Periphery.sol#L509
https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/Periphery.sol#L861
https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/Periphery.sol#L908

## Tool used

Manual Review

## Recommendation

The function `safeApprove` function has been deprecated by OpenZeppelin team. The new propesal is using `safeIncreaseAllowance` and `safeDecreaseAllowance` but since USDT is one of the supported tokens, there is a problem with this approach as well. Some tokens, like USDT (link below) have a built-in approval race condition protection, which makes them revert on setting a non-zero & non-max allowance, unless the allowance is already zero. The recommended way is to use the plain `approve` function.
https://gist.github.com/plutoegg/a8794a24dfa84d0b0104141612b52977#file-tethertoken-sol-L265-L274
