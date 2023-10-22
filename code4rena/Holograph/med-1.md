# [M-01] Using the `transfer()` function of address payable is discouraged

## Impact

The `transfer()` function only allows the recipient to use 2300 gas. If the recipient uses more than that, transfers will fail.

## Proof of Concept

_There are **1** instances of this issue:_

```solidity
File: /contracts/HolographOperator.sol

596: payable(hToken).transfer(hlgFee);
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

## Recommended Mitigation Steps

It is recommended using `call()` instead of `transfer()` to withdraw the contract's balance because `call()` will forward all available gas instead of only 2300 gas.
