# [M-02] Non-Compliant Allowance Logic

## Summary

Check the USDT `approve` function implementation:

https://gist.github.com/plutoegg/a8794a24dfa84d0b0104141612b52977#file-tethertoken-sol-L265-L274

## Vulnerability Detail

USDT has a built-in approval race condition protection, which makes them revert to setting a non-zero & non-max allowance unless the allowance is already zero.

## Impact

Since one of the external tokens is TetherToken, the current implementation will revert on setting a non-zero & non-max allowance, if the allowance is not already zero.

## Code Snippet

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Providers/AaveProvider.sol#L28

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Providers/BetaProvider.sol#L27

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Providers/CompoundProvider.sol#L35

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Providers/IdleProvider.sol#L28

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Providers/TruefiProvider.sol#L27

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/MainVault.sol#L313

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/libraries/Swap.sol#L44

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/libraries/Swap.sol#L69

## Tool used

Manual Review

## Recommendation

Avoid using `safeIncreaseAllowance` and `safeDecreaseAllowance`, use the standard `approve` to fit with the USDT built-in approval race condition protection.
