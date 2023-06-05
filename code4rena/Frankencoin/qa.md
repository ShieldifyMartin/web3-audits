## Introduction

Frankencoin QA report was done by martin and anonresercher, with a main focus on the low severity and non-critical security aspects of the implementaion and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

# Findings Summary

| ID      | Title                                                 | Severity     |
| ------- | ----------------------------------------------------- | ------------ |
| [L-01]  | Wrong checks                                          | Low          |
| [L-02]  | Event not emitted for important state change          | Low          |
| [L-03]  | Same event was called for different actions           | Low          |
| [L-04]  | Position with Address(0x0) can be registered          | Low          |
| [NC-01] | Misleading name and comment                           | Non-Critical |
| [NC-02] | Useless check                                         | Non-Critical |
| [NC-03] | `Ownable` and `MathUtil` should be marked as abstract | Non-Critical |

### [L-01] Wrong checks

The second condition (`totalSupply() > 0`) is not in place and should be separated with a different custom error or completely removed.

```solidity
main/contracts/Frankencoin.sol

84: if (_applicationPeriod < MIN_APPLICATION_PERIOD && totalSupply() > 0) revert PeriodTooShort();
85: if (_applicationFee < MIN_FEE  && totalSupply() > 0) revert FeeTooLow();
```

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Frankencoin.sol

### [L-02] Event not emitted for important state change

Consider adding an event for position registration (for example `PositionRegistered`).

```solidity
main/contracts/Frankencoin.sol

125: function registerPosition(address _position) override external {
```

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Frankencoin.sol

### [L-03] Same event was called for different actions

`constructor` and `initializeClone` both emit the `PositionOpened` event which is wrong, consider adding a separate event for the `initializeClone` function.

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Position.sol

### [L-04] Position with Address(0x0) can be registered

Wrong data can be added by everyone in the `positions` mapping.

```solidity
main/contracts/Frankencoin.sol

125: function registerPosition(address _position) override external {
```

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Frankencoin.sol

### [NC-01] Misleading name and comment

Even when the second part of the check is false the `msg.sender` is still a minter. Consider renaming the function and rewriting the comment.

```solidity
main/contracts/Frankencoin.sol

290: /**
291: * Returns true if the address is an approved minter.
292: */
293: function isMinter(address _minter) override public view returns (bool){
294:    return minters[_minter] != 0 && block.timestamp >= minters[_minter];
295: }
```

`isPosition` is really bad naming, suggesting a boolean to be returned. Consider renaming the function.

```solidity
main/contracts/Frankencoin.sol

300: function isPosition(address _position) override public view returns (address){
```

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Frankencoin.sol

### [NC-02] Useless check

Usage of the `minterOnly` modifier is recommended.

```solidity
main/contracts/Frankencoin.sol

125: function registerPosition(address _position) override external {

126: if (!isMinter(msg.sender)) revert NotMinter();
```

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Frankencoin.sol#L83

### [NC-03] `Ownable` and `MathUtil` should be marked as abstract

Contracts must be marked as abstract because we only use their functions by inheriting the contract and not instantiating it.

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/Ownable.sol

https://github.com/code-423n4/2023-04-frankencoin/blob/main/contracts/MathUtil.sol
