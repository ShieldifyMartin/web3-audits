# [M-01] Return values of `transfer()`/`transferFrom()` not checked

https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/PaprController.sol#L101
https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/PaprController.sol#L514
https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/PaprController.sol#L515
https://github.com/with-backed/papr/blob/9528f2711ff0c1522076b9f93fba13f88d5bd5e6/src/PaprController.sol#L546

## Impact

Some tokens do not return a bool (e.g. USDT, BNB, OMG) on ERC20 methods. But since the require-statement expects a bool, for such a token a void return will also cause a revert, despite an otherwise successful transfer. That is, the token payout will always revert for such tokens.

## Proof of Concept

```solidity
    collateralArr[i].addr.transferFrom(msg.sender, address(this), collateralArr[i].id);

    underlying.transfer(params.swapFeeTo, fee);

    underlying.transfer(proceedsTo, amountOut - fee);

    underlying.transfer(params.swapFeeTo, amountIn * params.swapFeeBips / BIPS_ONE);

    papr.transfer(auction.nftOwner, totalOwed - debtCached);
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Perform the checks and revert if necessary
