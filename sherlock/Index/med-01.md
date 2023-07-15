# [M-01] Usage of deprecated `transfer` to send eth

## Summary

Usage of deprecated way to send the entire Ether balance can lead to reverting `withdrawEtherBalance()` method.

## Vulnerability Detail

The use of the deprecated `transfer()` function for an address will inevitably make the transaction fail when:

1. The claimer smart contract does not implement a payable function.
2. The claimer smart contract does implement a payable fallback which uses more than 2300 gas unit.
3. The claimer smart contract implements a payable fallback function that needs less than 2300 gas units but is called through proxy, raising the call's gas usage above 2300. Additionally, using higher than 2300 gas might be mandatory for some multisig wallets. Even if the current design is the operator to use an EOA, you might decide to use a multisig in a later stage, and using `call()` will work for both, unlike `transfer()`.

## Impact

Usage of deprecated way to send the entire Ether balance can lead to reverting `withdrawEtherBalance()` method.

## Code Snippet

https://github.com/sherlock-audit/2023-05-Index-martin-petrov03/blob/main/index-coop-smart-contracts/contracts/adapters/AaveLeverageStrategyExtension.sol#L584

https://github.com/sherlock-audit/2023-05-Index-martin-petrov03/blob/main/index-coop-smart-contracts/contracts/adapters/AaveLeverageStrategyExtension.sol#L1221

## Tool used

Manual Review

## Recommendation

Use `call()` with value instead of `transfer()`.

```diff
-- msg.sender.transfer(address(this).balance);

++ (bool success, ) = msg.sender.call{value: address(this).balance}("");
++ require(success, "Transfer failed");0.
```
