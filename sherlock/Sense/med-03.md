# [M-03] Usage of deprecated `transfer` to send Ether

## Summary

Using a deprecated and insecure way to send Ether

## Vulnerability Detail

Using a deprecated and insecure way to send Ether

## Impact

The use of the deprecated `transfer()` function for an address will inevitably make the transaction fail when:

1. The claimer smart contract does not implement a payable function.
2. The claimer smart contract does implement a payable fallback which uses more than 2300 gas unit.
3. The claimer smart contract implements a payable fallback function that needs less than 2300 gas units but is called through proxy, raising the call's gas usage above 2300.
   Additionally, using higher than 2300 gas might be mandatory for some multisig wallets.
   Even if the address is EOA, it might be a multisig, and using `call()` will work for both, unlike `transfer()`.

## Code Snippet

```solidity
1013: payable(receiver).transfer(amt)
```

https://github.com/sherlock-audit/2023-03-sense-martin-petrov03/blob/main/sense-v1/pkg/core/src/Periphery.sol#L1013

## Tool used

Manual Review

## Recommendation:

Use `call()` with value instead of `transfer()`

```diff
- address(token) == ETH ? payable(receiver).transfer(amt) : token.safeTransfer(receiver, amt);

+ if(address(token) == ETH) {
+   (bool success, ) = receiver.call{value: amt}("");
+   require(success, "Transfer failed.");
+ } else {
+   token.safeTransfer(receiver, amt);
+ }
```
