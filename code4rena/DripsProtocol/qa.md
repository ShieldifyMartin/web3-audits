# Drips Protocol

## QA Report

### L-01 Input validation should be at the function beginning

It this way when input params are wrong, the verification would be done and then it will revert. Therefore, low severity matches.

```solidity
461: require(historyHash == finalHistoryHash, "Invalid drips history");
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Drips.sol

### L-02 Should be part of the `modifier` instead

```solidity
124: function _assertCallerIsDriver(uint32 driverId) internal view {
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/DripsHub.sol

```solidity
250: function _assertCurrSplits(uint256 userId, SplitsReceiver[] memory currReceivers)
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Splits.sol

### L-03 Extractable `splitsWeight` logic

Bad design decision and leads to waste gas.

```solidity
117: function _splitResult(uint256 userId, SplitsReceiver[] memory currReceivers, uint128 amount)

143: function _split(uint256 userId, uint256 assetId, SplitsReceiver[] memory currReceivers)

226: function _assertSplitsValid(SplitsReceiver[] memory receivers, bytes32 receiversHash) private {
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Splits.sol

### L-04 `emit` function called early

```solidity
215: emit SplitsSet(userId, newSplitsHash);
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Splits.sol

### L-05 Missing `nonReentrant` modifier `safeMint()`

`safeMint` does an unsafe external call to the recipient, so there is a reentrency attack vector. Make sure to add 'nonReentrant' modifier.

```solidity
85: _safeMint(to, tokenId);
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/NFTDriver.sol

### L-06 Incorrect check

Use `>=` instead, since in such case `weightSum` will have incorrect value.

```solidity
64: require(weightSum == totalSplitsWeight, "Invalid total receivers weight");
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/ImmutableSplitsDriver.sol

### N-01 Use a non-floating version of solidity

```solidity
File: /src/Drips.sol

File: /src/DripsHub.sol

File: /src/Splits.sol

File: /src/NFTDriver.sol

File: /src/Managed.sol

File: /src/Caller.sol

File: /src/AddressDriver.sol

File: /src/ImmutableSplitsDriver.sol
```

### N-02 Use elementary types or a user-defined type instead of a `struct` that has only one member

```solidity
173: struct DripsStorage {
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Drips.sol

```solidity
73: struct SplitsStorage {
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Splits.sol

### N-03 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

```solidity
357: uint32[2 ** 32] storage nextSqueezed = state.nextSqueezed[senderId];
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Drips.sol

### N-04 Unused imports

`DripsConfig` and `DripsConfigImpl` are imported but never actually used, consider removing them.

```solidity
4: import {Drips, DripsConfig, DripsHistory, DripsConfigImpl, DripsReceiver} from "./Drips.sol";
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/DripsHub.sol

`DripsHistory` is imported but never actually used, consider removing it.

```solidity
4: import {DripsHistory, DripsHub, DripsReceiver, SplitsReceiver, UserMetadata} from "./DripsHub.sol";
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/NFTDriver.sol

```solidity
5: DripsHistory,
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/AddressDriver.sol

### N-05 Copy pasting comments

Certainly not a good design decision, it takes hundreds of lines of code.

```solidity
/// It must preserve amounts, so if some amount of tokens is transferred to
/// an address, then later the same amount must be transferrable from that address.
/// Tokens which rebase the holders' balances, collect taxes on transfers,
/// or impose any restrictions on holding or transferring tokens are not supported.
/// If you use such tokens in the protocol, they can get stuck or lost.
/// @return amt The amount received but not split yet.
```

### N-06 Not using the named `return` variables anywhere in the `function` is confusing

Consider changing the variables to be an unnamed ones.

```solidity
300: function _receivableDripsCycles(uint256 userId, uint256 assetId)

475: ) private view returns (uint128 squeezedAmt) {

508: function _dripsState(uint256 userId, uint256 assetId)

538: ) internal view returns (uint128 balance) {

685: ) private view returns (uint32 maxEnd) {

742: ) private view returns (bool isEnough) {

795: returns (uint256 newConfigsLen)

837: returns (bytes32 dripsHash)

857: ) internal pure returns (bytes32 dripsHistoryHash) {

973: returns (uint32 start, uint32 end)

991: ) private pure returns (uint32 start, uint32 end_) {

1120: function _cycleOf(uint32 timestamp) private view returns (uint32 cycle) {

1128: function _currTimestamp() private view returns (uint32 timestamp) {

1134: function _currCycleStart() private view returns (uint32 timestamp) {
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Drips.sol

```solidity
145: function driverAddress(uint32 driverId) public view returns (address driverAddr) {

160: function nextDriverId() public view returns (uint32 driverId) {

169: function cycleSecs() public view returns (uint32 cycleSecs_) {

181: function totalBalance(IERC20 erc20) public view returns (uint256 balance) {

199: returns (uint32 cycles)

317: function splittable(uint256 userId, IERC20 erc20) public view returns (uint128 amt) {

331: returns (uint128 collectableAmt, uint128 splitAmt)

372: function collectable(uint256 userId, IERC20 erc20) public view returns (uint128 amt) {

432: function dripsState(uint256 userId, IERC20 erc20)

465: ) public view returns (uint128 balance) {

546: function hashDrips(DripsReceiver[] memory receivers) public pure returns (bytes32 dripsHash) {

587: function splitsHash(uint256 userId) public view returns (bytes32 currSplitsHash) {

598: returns (bytes32 receiversHash)
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/DripsHub.sol

```solidity
106: function _splittable(uint256 userId, uint256 assetId) internal view returns (uint128 amt) {

176: function _collectable(uint256 userId, uint256 assetId) internal view returns (uint128 amt) {

262: function _splitsHash(uint256 userId) internal view returns (bytes32 currSplitsHash) {

273: returns (bytes32 receiversHash)
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Splits.sol
