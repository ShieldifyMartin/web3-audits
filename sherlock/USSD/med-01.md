# [M-01] Hard-coded price feed addresses

## Summary

Hard-coded price feed, WETH, and DAI addresses

## Vulnerability Detail

Oracle price feed addresses can become unavailable, deprecated, or compromised, and the functionality of the smart contract may be affected.

## Impact

If an address is hard coded and becomes incorrect, it can lead to unexpected behavior or even loss of funds.

## Code Snippet

https://github.com/sherlock-audit/2023-05-USSD-martin-petrov03/blob/main/ussd-contracts/contracts/oracles/StableOracleDAI.sol#L24

https://github.com/sherlock-audit/2023-05-USSD-martin-petrov03/blob/main/ussd-contracts/contracts/oracles/StableOracleDAI.sol#L36

## Tool used

Manual Review

## Recommendation

Not only price feed addresses but also DAI and WETH addresses should pass it in the constructor as parameters.
