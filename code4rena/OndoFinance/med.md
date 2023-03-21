# [M-01] Non-Conforming ERC20 Tokens Not Recoverable

https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cCash/CCash.sol#L160
https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cCash/CCash.sol#L201
https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cCash/CCash.sol#L241
https://github.com/code-423n4/2023-01-ondo/blob/main/contracts/lending/tokens/cToken/CErc20.sol#L160

## Impact

There are functions to rescue any ERC20 tokens that were accidentally sent to the contract. However, there are tokens that do not return a value on success, which will cause the call to revert, even when the transfer would have been successful. This means that those tokens will be stuck forever and not be recoverable.

## Proof Of Concept

Someone accidentally transfers USDT, one of the most commonly used ERC20 tokens, to the contract. Because USDT’s transfer does not return a boolean, it will not be possible to recover those tokens and they will be stuck forever.

```solidity
160: token.transfer(admin, balance);

201: token.transferFrom(from, address(this), amount);

241: token.transfer(to, amount);
```

```solidity
160: token.transfer(admin, balance);

201: token.transferFrom(from, address(this), amount);

241: token.transfer(to, amount);
```

## Recommended Mitigation Steps

Use OpenZeppelin’s safeTransfer.
