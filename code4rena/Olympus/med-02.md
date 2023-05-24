[M-02] Deprecated `safeApprove()`

## Summary

The function `safeApprove` function has been deprecated by OpenZeppelin team.

## Vulnerability Detail

Using the deprecated `safeApprove` function

## Impact

Using officially deprecated functions could lead to vulnerabilities.

## Code Snippet

```solidity
69: ohm.safeApprove(address(MINTR), type(uint256).max);
```

https://github.com/sherlock-audit/2023-02-olympus-martin-petrov03/blob/main/src/policies/Burner.sol

## Tool used

Manual Review

## Recommendation

Use increase/decrease allowance instead.
