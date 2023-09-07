# [M-01] Users reward will be stuck when contract is paused

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol#L168

## Impact

Owner can pause not only inbound, but outbound functionality. When protocol is paused from the admin, all user's rewards will be stuck. It's a best practice to be able to pause inbound functionality but not outbound (withdraw) methods.

## Proof of Concept

```solidity
168: function getReward(
```

## Tools Used

Manual review

## Recommended Mitigation Steps

It's highly recommended to make this function active even while the contract is paused and let users be able to withdraw their rewards. There is no external calls in this function, so consider removing the `nonReentrant` modifier as well.
