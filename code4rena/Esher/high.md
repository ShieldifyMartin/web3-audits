# [H-01] Contract could get locked, resulting in 100% funds loss

https://github.com/code-423n4/2022-12-escher/blob/main/src/minters/FixedPrice.sol#L109
https://github.com/code-423n4/2022-12-escher/blob/main/src/minters/OpenEdition.sol#L92

## Impact

The recommended way to send ether is with `call` function. Using `transfer` could lead to running out of gas, due to the fact that it is predefined and the transfer will fail, in such scenario there won't be way to withraw any funds from the contract. Transfer may fail because there is not enough ether on sender contract's balance, or because recipient is a smart contract that does not accept payments, or needs more than 2300 gas in order to process incoming ether transfers. That's why I think high severity matches well.

## Proof of Concept

```solidity
ISaleFactory(factory).feeReceiver().transfer(address(this).balance / 20);
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Use `call` instead. For example

```solidity
    (bool success, ) = ISaleFactory(factory).feeReceiver().call{value: address(this).balance / 20}("");
    require(success, "Transfer failed.");
```
