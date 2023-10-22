# M-01 Return values of `transfer()`/`transferFrom()` not checked

## Impact

Not all IERC20 implementations revert() when there’s a failure in transfer() / transferFrom(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

## Proof of Concept

_There are **4** instances of this issue:_

```solidity
File: /contracts/HolographOperator.sol

400: _utilityToken().transfer(job.operator, leftovers);

596: payable(hToken).transfer(hlgFee);
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/enforcer/PA1D.sol

416: require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

439: require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

## Recommended Mitigation Steps

Perform the check.
