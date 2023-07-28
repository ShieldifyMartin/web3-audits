# [M-01] Incomplete pausing logic

## Summary

Functions missing the `whenNotPaused` modifier

## Vulnerability Detail

Important state data can still be updated even when some emergency happened and `TellerV2` was paused.

## Impact

The `whenNotPaused` modifier is used for the following methods: `submitBid`, `lenderAcceptBid`, and `claimLoanNFT`. However, there are other state-changing functions missing it.

## Code Snippet

```solidity
function cancelBid(uint256 _bidId) external {
```

https://github.com/sherlock-audit/2023-03-teller-martin-petrov03/blob/main/teller-protocol-v2/packages/contracts/contracts/TellerV2.sol#L417

## Tool used

Manual Review

## Recommendation

It's highly recommended to add it to `cancelBid`, `marketOwnerCancelBid`, `repayLoanMinimum`, `repayLoanFull`, `repayLoan`, and `liquidateLoanFull` functions.
