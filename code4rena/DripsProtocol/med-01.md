# [M-01] Centralisation Vulnerability

https://github.com/code-423n4/2023-01-drips/blob/main/src/DripsHub.sol#L386
https://github.com/code-423n4/2023-01-drips/blob/main/src/DripsHub.sol#L409
https://github.com/code-423n4/2023-01-drips/blob/main/src/NFTDriver.sol#L109
https://github.com/code-423n4/2023-01-drips/blob/main/src/NFTDriver.sol#L133
https://github.com/code-423n4/2023-01-drips/blob/main/src/NFTDriver.sol#L254
https://github.com/code-423n4/2023-01-drips/blob/main/src/NFTDriver.sol#L263
https://github.com/code-423n4/2023-01-drips/blob/main/src/NFTDriver.sol#L277
https://github.com/code-423n4/2023-01-drips/blob/main/src/AddressDriver.sol#L60
https://github.com/code-423n4/2023-01-drips/blob/main/src/AddressDriver.sol#L76

## Impact

It's a centralisation vulnerability if an owner or pauser can pause not only inbound, but outbound functionality. It's a best practice to be able to pause inbound (`registerDriver`, `updateDriverAddress`, `setDrips`, `setSplits`, `emitUserMetadata`, etc.) functionality but not outbound (`collect`, `give`) methods

## Proof of Concept

```solidity
function collect(uint256 userId, IERC20 erc20)
    public
    whenNotPaused
    onlyDriver(userId)
    returns (uint128 amt)

function give(uint256 userId, uint256 receiver, IERC20 erc20, uint128 amt)
    public
    whenNotPaused
    onlyDriver(userId)
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Remove pausability (`whenNotPaused` modifier) from outbound functions.
