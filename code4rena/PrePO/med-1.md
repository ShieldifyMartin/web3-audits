# [M-01] Usage of deprecated `send` to send eth

https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/DepositHook.sol#L50
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/RedeemHook.sol#L22
https://github.com/prepo-io/prepo-monorepo/blob/feat/2022-12-prepo/apps/smart-contracts/core/contracts/WithdrawHook.sol#L77

## Impact

The recommended way to send ether is with `call` function. Using `transfer`, `send` or `sendValue` could lead to running out of gas, due to the fact that it is predefined and the transfer will fail, in such scenario there won't be way to withraw the amount from the contract.

## Proof of Concept

```solidity
50: _tokenSender.send(_sender, _fee);
```

```solidity
22: _tokenSender.send(sender, fee);
```

```solidity
77: _tokenSender.send(_sender, _fee);
```

## Tools Used

Manual audit

## Recommended Migration Steps

Use call instead. For example:

```solidity
(bool success, ) = _tokenSender.call{value: _fee}("");
require(success, "Transfer failed.");
```
