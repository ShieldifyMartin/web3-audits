# Invalid `deadline` parameter by swap

## Summary

**Likelihood:**
Medium, because it requires certain market conditions

**Impact:**
High, because it might lead to fund loss for the protocol

## Vulnerability Detail

The `deadline` parameter enforces a time limit by which the transaction must be executed. This parameter was set to `block.timestamp` and then it was commented, which might lead to a case, in which the validator can hold the transaction, and the block it is eventually put into will be block.timestamp, so this offers no protection. Without it, the transaction may sit in the mempool and be executed at a much later time potentially resulting in a worse price. Additionally, there is no slippage protection (`amountOutMinimum: 0`). This in combination exposes the protocol to a potential critical loss of funds.

## Impact

The protocol is exposed to a potential critical loss of funds due to invalid parameters.

## Code Snippet

```solidity
IV3SwapRouter.ExactInputParams memory params = IV3SwapRouter
    .ExactInputParams({
        path: _path,
        recipient: address(this),
        //deadline: block.timestamp,
        amountIn: _sellAmount,
        amountOutMinimum: 0
    });
```

https://github.com/sherlock-audit/2023-05-USSD-martin-petrov03/blob/main/ussd-contracts/contracts/USSD.sol#L231

## Tool used

Manual Review

## Recommendation

Introduce a correct `deadline` parameter. Also, add a valid slippage amount, but be aware that hard coding may lead to frozen owner funds.
