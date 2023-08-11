# [M-01] Usage of deprecated `transfer` to send Ether

## Summary

Usage of deprecated way to transfer ether to the owner account.

## Vulnerability Detail

Usage of deprecated and not extendable way to transfer ether to the owner account.

## Impact

The use of the deprecated `transfer()` function for an address will inevitably make the transaction fail when using multisig wallet. Additionally, using higher than 2300 gas might be mandatory for some multisig wallets.
Even when the current design is for using owner EOA you might decide to use a multisig in a later stage, and using `call()` will work for both, unlike `transfer()`.

## Code Snippet

```solidity
287: payable(owner()).transfer(address(this).balance);
```

https://github.com/sherlock-audit/2023-02-bond-martin-petrov03/blob/main/bonds/src/BondBatchAuctionV1.sol#L287

## Tool used

Manual Review

## Recommendation

Use `call()` with value instead of `transfer()`

```diff
- payable(owner()).transfer(address(this).balance);

+ (bool success, ) = payable(owner()).call{value: address(this).balance}("");
+ require(success, "Transfer failed.");
```
