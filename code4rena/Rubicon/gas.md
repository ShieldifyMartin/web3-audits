## Introduction

Rubicon GAS report was done by martin and anonresercher, with a main focus on the low severity and non-critical security aspects of the implementation and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

# Findings Summary

| ID     | Title                                                                                                       | Severity |
| ------ | ----------------------------------------------------------------------------------------------------------- | -------- |
| [G-01] | <x> = <x> + <y> costs less gas than <x> += <y> for state variables, same as <x> = <x> - <y> than <x> -= <y> | GAS      |
| [G-02] | ++x costs less gas than x++ for state variables. The same change can be applied to x-- as well.             | GAS      |
| [G-03] | Not using the named return variables when a function returns wastes deployment gas                          | GAS      |
| [G-04] | Replace x <= y with x < y + 1, and x >= y with x > y - 1                                                    | GAS      |
| [G-05] | Splitting `require()` statements that use `&&` saves gas                                                    | GAS      |
| [G-06] | Expressions for constant values such as a call to keccak256(), should use immutable rather than constant    | GAS      |
| [G-07] | Avoid using state variable in emit (130 gas)                                                                | GAS      |
| [G-08] | Change public state variable visibility to private                                                          | GAS      |

## [G-01] <x> = <x> + <y> costs less gas than <x> += <y> for state variables, same as <x> = <x> - <y> than <x> -= <y>

**There are 5 instances of this issue:**

```solidity
178: vars.currentBathTokenAmount += _borrowLoop( asset, quote, bathTokenAsset, bathTokenQuote, vars.currentAssetBalance, vars.toBorrow );

418: positions[_positionId].bathTokenAmount += _bathTokenAmount;

545: _assetAmount += _loopBorrowed;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

```solidity
571: _amount -= mul(amount, feeBPS) / 100_000;

574: _amount -= mul(amount, makerFee()) / 100_000;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-02] ++x costs less gas than x++ for state variables. The same change can be applied to x-- as well.

This change would save up to 6 gas per instance.

**There are 2 instances of this issue:**

```solidity
1349: _span[address(pay_gem)][address(buy_gem)]++;

1383: _span[pay_gem][buy_gem]--;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-03] Not using the named return variables when a function returns wastes deployment gas

**There are 11 instances of this issue:**

```solidity
58: function min(uint256 x, uint256 y) internal pure returns (uint256 z) {

62: function max(uint256 x, uint256 y) internal pure returns (uint256 z) {

66: function imin(int256 x, int256 y) internal pure returns (int256 z) {

70: function imax(int256 x, int256 y) internal pure returns (int256 z) {

276: function isActive(uint256 id) public view returns (bool active) {

280: function getOwner(uint256 id) public view returns (address owner) {

284: function getRecipient(uint256 id) public view returns (address owner) {

288: function getOffer( uint256 id ) public view returns (uint256, ERC20, uint256, ERC20) {

486: function make( ERC20 pay_gem, ERC20 buy_gem, uint128 pay_amt, uint128 buy_amt ) external virtual returns (bytes32 id) {

608: function isClosed() public pure returns (bool closed) {

843: function cancel( uint256 id ) public override can_cancel(id) returns (bool success) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-04] Replace x <= y with x < y + 1, and x >= y with x > y - 1

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

**There are 19 instances of this issue:**

```solidity
51: require((z = x - y) <= x, "ds-math-sub-underflow");

47: require((z = x + y) >= x, "ds-math-add-overflow");

59, 67: return x <= y ? x : y;

63, 71: return x >= y ? x : y;

807: require(_dust[address(pay_gem)] <= pay_amt);

1008: if (pay_amt >= offers[offerId].buy_amt) {

1024: require(fill_amt >= min_fill_amount, "min_fill_amount isn't filled");

1048: if (buy_amt >= offers[offerId].pay_amt) {

1066: require(fill_amt <= max_fill_amount, "fill_amt exceeds max_fill_amount");

1220: return mul(offers[low].buy_amt, offers[high].pay_amt) >= mul(offers[high].buy_amt, offers[low].pay_amt);

1275: if ( t_buy_amt > 0 && t_pay_amt > 0 && t_pay_amt >= _dust[address(t_pay_gem)] ) {
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
195: if (block.timestamp >= periodFinish[address(rewardsToken)]) {

217: require( rewardRates[address(rewardsToken)] <= balance.div(rewardsDuration[address(rewardsToken)]), "Provided reward too high" );
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```
282: require(_amountToRepay <= _quoteBalance, "_repay: balance of quote lt. debt" );

528: while (_assetAmount <= _desiredAmount) {

576: require(_leverage > _wad && _leverage <= _leverageMax, "_leverageCheck{Long}: INVLAID LEVERAGE" )

580: require(_leverage >= _wad && _leverage <= _leverageMax, "_leverageCheck{Short}: INVLAID LEVERAGE" );
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## [G-05] Splitting `require()` statements that use `&&` saves gas

Instead of using `&&` on single require check using two require checks can save gas

**There are 11 instances of this issue:**

```solidity
348: if (_offer.owner == address(0) && getRecipient(id) != address(0)) {

865: require(payAmts.length == payGems.length && payAmts.length == buyAmts.length && payAmts.length == buyGems.length, "Array lengths do not match");

902: require(!isActive(id) && _rank[id].delb != 0 && _rank[id].delb < block.number - 10);

1153: if (isActive(id) && offers[id].pay_amt < _dust[address(offers[id].pay_gem)]) {

1275:  if (t_buy_amt > 0 && t_pay_amt > 0 && t_pay_amt >= _dust[address(t_pay_gem)]) {

1361: require(_rank[id].delb == 0 && isOfferSorted(id));
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

```solidity
95: require( msg.sender == myBathTokenBuddy && msg.sender != address(0) && friendshipStarted, "You are not my buddy!" );
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/periphery/BathBuddy.sol

```solidity
170: if (i.add(1) == vars.limit && vars.lastBorrow != 0) {

378: if (IERC20(_bathTokenAsset).balanceOf(address(this)) == 0 && borrowBalance(_bathTokenAsset) == 0 ) {

576: require(_leverage > _wad && _leverage <= _leverageMax, "_leverageCheck{Long}: INVLAID LEVERAGE" )

580: require(_leverage >= _wad && _leverage <= _leverageMax, "_leverageCheck{Short}: INVLAID LEVERAGE" );
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/utilities/poolsUtility/Position.sol

## [G-06] Expressions for constant values such as a call to keccak256(), should use immutable rather than constant

It is expected that the value should be converted into a constant value at compile time. But actually the expression is re-calculated each time the constant is referenced./

**There are **1** instances of this issue:**

```solidity
232: bytes32 internal constant MAKER_FEE_SLOT = keccak256("WOB_MAKER_FEE");
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol

## [G-07] Avoid using state variable in emit (130 gas)

Using a state variable in LogSetOwner emits wastes gas.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L27

If the following recommendation is taken into account, 130 gas is saved.

```solidity
function setOwner(address owner_) external auth {
  emit LogSetOwner(owner_);
  owner = owner_;
}
```

## [G-08] Change public state variable visibility to private

If it is preferred to change the visibility of the owner state variable to private, this will save significant gas.

1 result - 1 file:

```solidity
23: address public owner;
```

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L23
