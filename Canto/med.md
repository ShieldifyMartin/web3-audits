# M-01 Usage of deprecated `sendValue` to send eth

https://github.com/code-423n4/2022-11-canto/blob/main/CIP-001/src/Turnstile.sol#L141

## Impact

The recommended way to send ether is with `call` function. Using `transfer` or `sendValue` could lead to running out of gas, due to the fact that it is predefined and the transfer will fail, in such scenario there won't be way to withraw the amount from the contract.

## Proof of Concept

```solidity
141: Address.sendValue(_recipient, _amount);
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Use call instead. For example

```solidity
(bool success, ) = _recipient.call{amount}("");
if(!success) {
    revert TransferFailed();
}
```
