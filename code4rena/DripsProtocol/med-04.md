# [M-04] Balance manipulation when contract is paused

https://github.com/code-423n4/2023-01-drips/blob/main/src/DripsHub.sol#L629
https://github.com/code-423n4/2023-01-drips/blob/main/src/DripsHub.sol#L635

## Impact

State-changing methods missing the `whenNotPaused()` modifier, is a security hole. Even when contract is paused `_increaseTotalBalance` and `_decreaseTotalBalance` methods can be called internally. Therefore, medium severity matches.

## Proof of Concept

```solidity
function _increaseTotalBalance(IERC20 erc20, uint128 amt) internal {
    mapping(IERC20 => uint256) storage totalBalances = _dripsHubStorage().totalBalances;
    require(totalBalances[erc20] + amt <= MAX_TOTAL_BALANCE, "Total balance too high");
    totalBalances[erc20] += amt;
}

function _decreaseTotalBalance(IERC20 erc20, uint128 amt) internal {
    _dripsHubStorage().totalBalances[erc20] -= amt;
}
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Add `whenNotPaused` modifier to these functions.
