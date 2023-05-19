# [M-01] `executeCalls` function may fail due to exceed gas limit

https://github.com/pooltogether/ERC5164/blob/5647bd84f2a6d1a37f41394874d567e45a97bf48/src/libraries/CallLib.sol#L61

## Impact

The function `executeCalls` in `CallLib` may fail due to unbounded loop over `_callsLength` can get very large due to the function input parameter.
And the loop in `executeCalls` did not have a mechanism to stop, it’s only based on the length `_callsLength`, and may take all the gas limit. If the gas limit is reached, this transaction will fail or revert. That why I think Medium matches correctly.

## Proof of Concept

```solidity
for (uint256 _callIndex; _callIndex < _callsLength; _callIndex++) {
    Call memory _call = _calls[_callIndex];

    (bool _success, bytes memory _returnData) = _call.target.call(
    abi.encodePacked(_call.data, _nonce, _sender)
    );

    if (!_success) {
    revert CallFailure(_callIndex, _returnData);
    }
}
```

## Tools Used

Manual audit

## Recommended Migration Steps

Add the coresponding check and revert with custom error.
