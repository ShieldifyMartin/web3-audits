# [M-03] An important flow of actions is not enforced, just documented

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L232

## Impact

This can easily be forgotten or missed when executing a call to the method. This way of ensuring proper flow of operations is used is error-prone.

## Proof of Concept

The NatSpec of BathBuddy::setRewardsDuration contains the following comment:

```solidity
    // This must be set before notifying a new rewards program for a given token
    // Must be used before? notifyRewardAmount to set the new period
```

## Tools Used

Manual review

## Recommendation

Ensure that `notifyRewardAmount` was called beforehand by using a flag or some storage variable to be certain that admin won't call setRewardsDuration.
