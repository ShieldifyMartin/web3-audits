# [M-01] Missing function

## Summary

Missing function

## Vulnerability Detail

There is no way to delete protocol from `protocolBlacklist` mapping. There are only `getProtocolBlacklist` and `setProtocolBlacklist` functions. Vault-whitelisted users can blacklist protocol by mistake and there is no way to correct this mistake.

## Impact

There is a unrecoverable `setProtocolBlacklist` operation.

## Code Snippet

```solidity
mapping(uint256 => mapping(uint256 => bool)) public protocolBlacklist;
```

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Controller.sol#L26

```solidity
function getProtocolBlacklist(
    uint256 _vaultNumber,
    uint256 _protocolNum
  ) external view override onlyVault returns (bool) {
    return protocolBlacklist[_vaultNumber][_protocolNum];
}
```

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Controller.sol#L97

```solidity
function setProtocolBlacklist(
    uint256 _vaultNumber,
    uint256 _protocolNum
  ) external override onlyVault {
    protocolBlacklist[_vaultNumber][_protocolNum] = true;
}
```

https://github.com/sherlock-audit/2023-01-derby-martin-petrov03/blob/main/derby-yield-optimiser/contracts/Controller.sol#L117

## Tool used

Manual Review

## Recommendation

Add `removeProtocolBacklist` function that perform delete operation (delete protocolBlacklist[\_vaultNumber][_protocolnum]).
