# [M-01] `emitUserMetadata` function may fail due to exceed gas limit

https://github.com/code-423n4/2023-01-drips/blob/main/src/DripsHub.sol#L613

## Impact

The function `emitUserMetadata` in `DripsHub` may fail due to unbounded loop over `userMetadata` can be very large due to the user input. However, function could be called only from drivers, it's still public and large array could be passed.
And the loop in `emitUserMetadata` did not have a mechanism to stop, it’s only based on the array length, and may take all the gas limit. If the gas limit is reached, this transaction will fail or revert.

## Proof of Concept

```solidity
for (uint256 i = 0; i < userMetadata.length; i++) {
    UserMetadata calldata metadata = userMetadata[i];
    emit UserMetadataEmitted(userId, metadata.key, metadata.value);
}
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Perform `userMetadata` length check and revert if necessary.
