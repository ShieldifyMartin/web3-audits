# [M-03] Non-Conforming ERC20 Tokens Not Recoverable

## Summary

Using not completely safe functions

## Vulnerability Detail

It does not provide any error checking or validation when transferring tokens and it could additionally lead to potential errors and losses when transferring tokens.

## Impact

The transfer could fail but the code would continue to execute.

## Code Snippet

```solidity
45: ICToken(_cToken).transfer(msg.sender, cTokensReceived);
```

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Providers/CompoundProvider.sol#L45

```solidity
39: IIdle(_iToken).transfer(msg.sender, tTokensReceived);
```

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Providers/IdleProvider.sol#L39

```solidity
38: ITruefi(_tToken).transfer(msg.sender, tTokensReceived);
```

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Providers/TruefiProvider.sol#L38

## Tool used

Manual Review

## Recommendation

`safeTransfer` function is available but not added in the interface, use it instead.
