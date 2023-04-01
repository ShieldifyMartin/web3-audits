# [M-01] `fund`: Non-Conforming ERC20 Tokens Not Recoverable

https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LQTY/CommunityIssuance.sol#L103
https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LQTY/CommunityIssuance.sol#L127
https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LQTY/LQTYStaking.sol#L135
https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LQTY/LQTYStaking.sol#L171
https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LUSDToken.sol#L222
https://github.com/code-423n4/2023-02-ethos/blob/main/Ethos-Core/contracts/LUSDToken.sol#L237

## Impact

There is a function `fund` to rescue any ERC20 tokens that were accidentally sent to the contract. However, there are tokens that do not return a value on success, which will cause the call to revert, even when the transfer would have been successful. This means that those tokens will be stuck forever and not be recoverable.

## Proof of Concept

Someone accidentally transfers USDT, one of the most commonly used ERC20 tokens, to the contract. Because USDT’s transfer does not return a boolean, it will not be possible to recover those tokens and they will be stuck forever.

## Tools Used

Manual review

## Recommended Mitigation Steps

Perform the response check and revert if necessary.
