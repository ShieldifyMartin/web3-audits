# M-01 Return values of `transferFrom()` not checked

https://github.com/code-423n4/2022-11-redactedcartel/blob/main/src/PirexGmx.sol#L436

## Impact

1. Not all IERC20 implementations revert() when there’s a failure in transfer() / transferFrom(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment.
2. Since the IERC20 interface requires a boolean return value, attempting to transfer ERC20s with missing return values will revert. This means `${project name}` payment terminals cannot support a number of popular ERC20s, including USDT and BNB.

## Proof of Concept

```solidity
436: stakedGlp.transferFrom(msg.sender, address(this), amount);
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Perform the check and revert if transferFrom retruns false
