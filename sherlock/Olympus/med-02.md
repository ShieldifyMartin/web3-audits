# [M-02] Signature Malleability

## Summary

Replay attack vector

## Vulnerability Detail

Replay attack vector

## Impact

The function calls the Solidity `ecrecover()` function directly to verify the given signatures. However, the `ecrecover()` EVM opcode allows malleable (non-unique) signatures and thus is susceptible to replay attacks.
Although a replay attack seems not possible for this contract, I recommend using the battle-tested OpenZeppelin’s ECDSA library.

## Code Snippet

```solidity
253: address signer = ecrecover(hash, v, r, s);

866: address signer = ECDSA.recover(hash, v, r, s);
```

https://github.com/sherlock-audit/2023-03-olympus-martin-petrov03/blob/main/sherlock-olympus/src/external/OlympusERC20.sol#L253
https://github.com/sherlock-audit/2023-03-olympus-martin-petrov03/blob/main/sherlock-olympus/src/external/OlympusERC20.sol#L866

## Tool used

Manual Review

## Recommendation

Use the `ecrecover` function from OpenZeppelin’s ECDSA library for signature verification. (Ensure using a version > 4.7.3 for there was a critical bug >= 4.1.0 < 4.7.3).
