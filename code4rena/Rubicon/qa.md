## Introduction

Rubicon QA report was done by martin and anonresercher, with a main focus on the low severity and non-critical security aspects of the implementaion and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

# Findings Summary

| ID      | Title                                                                            | Severity     |
| ------- | -------------------------------------------------------------------------------- | ------------ |
| [L-01]  | Modifiers shouldn’t update state                                                 | Low          |
| [L-02]  | Event not emitted for important state change                                     | Low          |
| [L-03]  | Incorrect check                                                                  | Low          |
| [NC-01] | Not using the named `return` variables anywhere in the `function` is confusing   | Non-Critical |
| [NC-02] | Inconsistency in versions                                                        | Non-Critical |
| [NC-03] | NatSpec docs are incomplete                                                      | Non-Critical |
| [NC-04] | Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18) | Non-Critical |

## [L-01] Modifiers shouldn’t update state

It is generally considered a best practice in Solidity that modifiers should not update the state of the contract, because it can lead to unexpected behavior and can even introduce potential vulnerabilities. One of them is that updating state in a modifier can cause inconsistencies in the contract state in case the function it modifies fails. It also makes the code harder to debug.

```solidity
247: modifier updateReward(address account, address token) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

## [L-02] Event not emitted for important state change

The `OfferDeleted` event is commented and currently, there is no event for this action.
The `LogKill` event is changed with `LogItemUpdate` but it's still commented. Consider removing commented event code and adding a separate event for offer cancel action.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L438

Events named `MarginIncreased` and `TokensWithdrawn` should be emitted.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L243

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol#L226

## [L-03] Incorrect check

Avoid using strict checks for Ether amount.

```diff
-- require(msg.value == _totalAmount, "FeeWrapper: not enough ETH sent");
++ require(msg.value >= _totalAmount, "FeeWrapper: not enough ETH sent");
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/FeeWrapper.sol

## [NC-01] Not using the named `return` variables anywhere in the `function` is confusing

Consider changing the variable to be an unnamed one.

```solidity
276: function isActive(uint256 id) public view returns (bool active) {

280: function getOwner(uint256 id) public view returns (address owner) {

284: function getRecipient(uint256 id) public view returns (address owner) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [NC-02] Inconsistency in versions

Use the same stable recent version of solidity in all of the contracts. Use a stable pragma statement to get a deterministic bytecode. Also use latest Solidity version to get all compiler features, bugfixes and optimizations.

## [NC-03] NatSpec docs are incomplete

In almost all methods the NatSpec documentation is incomplete as it is missing parameters and return variables in it. NatSpec documentation is essential for better understanding of the code by developers and auditors and is strongly recommended. Please refer to the [NatSpec format](https://docs.soliditylang.org/en/v0.8.17/natspec-format.html) and follow the guidelines outlined there.

## [NC-04] Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

While the compiler knows to optimize away the exponentiation, it’s still better coding practice to use idioms that do not require compiler optimization, if they exist.

```solidity
74: uint256 constant WAD = 10 ** 18;

75: uint256 constant RAY = 10 ** 27;

1015: uint256 baux = rmul(mul(pay_amt, 10 ** 9), rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt) ) / 10 ** 9;

1055: fill_amt = add(fill_amt, rmul( mul(buy_amt, 10 ** 9), rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt) ) / 10 ** 9 );

1095: fill_amt = add(fill_amt, rmul( mul(pay_amt, 10 ** 9), rdiv(offers[offerId].pay_amt, offers[offerId].buy_amt) ) / 10 ** 9 );

1128: fill_amt = add(fill_amt, rmul( mul(buy_amt, 10 ** 9), rdiv(offers[offerId].buy_amt, offers[offerId].pay_amt) ) / 10 ** 9 );
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
309: _max = (_liq.mul(10 ** 18)).div(_price);

321: uint256 _interest = ( (_borrowedAmount).mul(borrowRate(_bathToken).mul(_blockDelta)) ).div(10 ** 18);
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol
