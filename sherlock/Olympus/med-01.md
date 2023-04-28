# [M-01] Single-step ownership transfer can be dangerous

## Summary

**Likelihood:**
Medium, because even a small mistake would lead to unchangeable kernel

**Impact:**
Medium, because it limits the functionality of the protocol

## Vulnerability Detail

Because of human mistake, it’s possible to set a new invalid kernel. In such case, there won't be a way to change it since `changeKernel` could be called only if the msg.sender is equal to address(kernel) which won't be the case anymore.

## Impact

It’s highly possible to make the kernel address unchangeable from the `changeKernel` function.

## Code Snippet

```solidity
96: function changeKernel(Kernel newKernel_) external onlyKernel {
```

https://github.com/sherlock-audit/2023-03-olympus-martin-petrov03/blob/main/sherlock-olympus/src/Kernel.sol#L96

## Tool used

Manual Review

## Recommendation

When you want to change the kernel address it’s better to propose a new kernel, and then accept this ownership with the new wallet. Implement Two-Factor Authentication with pending and approve functions.
