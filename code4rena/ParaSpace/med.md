# M-01 Use call instead

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/LiquidationLogic.sol#L875

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol#L571

## Impact

The recommended way to send ether is with `call` function. Using `transfer` or `sendValue` could lead to running out of gas, due to the fact that it is predefined and the transfer will fail, in such scenario there won't be way to withraw the amount from the contract.

## Proof of Concept

```solidity
875: Address.sendValue(
```

```solidity
571: Address.sendValue(payable(msg.sender), ethLeft);
```

## Tools Used

Manual audit

### Recommended Migration Steps

The recommended way to send ether is with `call` function. Using `transfer` or `sendValue` could lead to running out of gas, due to the fact that it is predefined and the transfer will fail, in such scenario there won't be way to withraw the amount from the contract.
