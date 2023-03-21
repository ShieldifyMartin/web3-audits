# [M-01] Return values of `transfer()`/`transferFrom()` not checked

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/LineLib.sol#L46
https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/LineLib.sol#L69

## Impact

Not all IERC20 implementations revert() when there’s a failure in transfer() / transferFrom(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually making a payment

## Proof of Concept

```solidity
46: IERC20(token).safeTransfer(receiver, amount);

69: IERC20(token).safeTransferFrom(sender, address(this), amount);
```

## Tools Used

VScode

## Recommended Migration Steps

Perform the checks.

# M-02 Usage of deprecated `transfer` to send eth

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/LineLib.sol#L48

## Impact

If gas costs are subject to change, then smart contracts can’t depend on any particular gas costs.
Any smart contract that uses transfer() or send() is taking a hard dependency on gas costs by forwarding a fixed amount of gas: 2300.

## Proof of Concept

```solidity
48: payable(receiver).transfer(amount);
```

## Tools Used

VScode

## Recommended Migration Steps

Use call instead and take care of reentrancy. For example

```solidity
    (bool success, ) = msg.sender.call{amount}("");
    require(success, "Transfer failed.");
```

# M-03 Use Two-Factor Authentication

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/SpigotLib.sol#L178

## Impact

A simple input mistake could lead to contract being out of control forever.

## Proof of Concept

```solidity
178: function updateOwner(SpigotState storage self, address newOwner) external returns (bool) {
```

## Tools Used

VScode

## Recommended Migration Steps

Implement Two-Factor Authentication with pending and approve ownership functions
