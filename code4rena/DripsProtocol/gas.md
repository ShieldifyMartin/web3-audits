# Drips Protocol

## Gas Optimizations Report

### G-01 Use elementary types or a user-defined type instead of a `struct` that has only one member

```solidity
173: struct DripsStorage {
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Drips.sol

```solidity
73: struct SplitsStorage {
```

https://github.com/code-423n4/2023-01-drips/blob/main/src/Splits.sol

### G-02 Not using the named `return` variables when a function returns wastes deployment gas

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
