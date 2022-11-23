# Juicebox

## Gas Optimizations Report

### G-01 `public` functions not called by the contract should be declared `external` instead

_There are **4** instances of this issue:_

```solidity
File: /contracts/JB721TieredGovernance.sol

147: function setTierDelegates(JBTiered721SetTierDelegatesData[] memory _setTierDelegatesData)
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JB721TieredGovernance.sol

```solidity
File: /contracts/JBTiered721DelegateStore.sol

499: function balanceOf(address _nft, address _owner) public view override returns (uint256 balance) {

523: function redemptionWeightOf(address _nft, uint256[] calldata _tokenIds)

550: function totalRedemptionWeight(address _nft) public view override returns (uint256 weight) {
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol

### G-02 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **7** instances of this issue:_

```solidity
File: /contracts/JBTiered721Delegate.sol

290: function mintFor(JBTiered721MintForTiersData[] memory _mintForTiersData)

321: function adjustTiers(JB721TierParams[] calldata _tiersToAdd, uint256[] calldata _tierIdsToRemove)

370: function setDefaultReservedTokenBeneficiary(address _beneficiary) external override onlyOwner {

386: function setBaseUri(string memory _baseUri) external override onlyOwner {

402: function setContractUri(string calldata _contractUri) external override onlyOwner {

418: function setTokenUriResolver(IJBTokenUriResolver _tokenUriResolver) external override onlyOwner {

480: function mintFor(uint16[] memory _tierIds, address _beneficiary)
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

### G-03 The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

This change would save up to 6 gas per instance/loop.

_There are **6** instances of this issue:_

```solidity
File: /contracts/JBTiered721DelegateStore.sol

1106: numberOfBurnedFor[msg.sender][_tierId]++;

1108: _storedTierOf[msg.sender][_tierId].remainingQuantity++;
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol

```solidity
File: /contracts/libraries/JBIpfsDecoder.sol

59: digitlength++;

68: for (uint256 i = 0; i < _length; i++) {

76: for (uint256 i = 0; i < _input.length; i++) {

84: for (uint256 i = 0; i < _indices.length; i++) {
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

### G-04 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **7** instances of this issue:_

```solidity
File: /contracts/JBTiered721DelegateStore.sol

354: supply += _storedTier.initialQuantity - _storedTier.remainingQuantity;

409: units += _balance * _storedTierOf[_nft][_i].votingUnits;

506: balance += tierBalanceOf[_nft][_owner][_i];

534: weight += _storedTierOf[_nft][tierIdOfToken(_tokenIds[_i])].contributionFloor;

563: weight +=

827: numberOfReservesMintedFor[msg.sender][_tierId] += _count;
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol

```solidity
File: /contracts/libraries/JBIpfsDecoder.sol

52: carry += uint256(digits[j]) * 256;
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

### G-05 Replace `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **1** instances of this issue:_

```solidity
File: /contracts/JBTiered721DelegateStore.sol

903: if (_storedTierOf[msg.sender][_tierId].lockedUntil >= block.timestamp) revert TIER_LOCKED();
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateStore.sol

### G-06 It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied

Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

_There are **5** instances of this issue:_

```solidity
File: /contracts/libraries/JBIpfsDecoder.sol

49: for (uint256 i = 0; i < _source.length; ++i) {

51: for (uint256 j = 0; j < digitlength; ++j) {

68: for (uint256 i = 0; i < _length; i++) {

76: for (uint256 i = 0; i < _input.length; i++) {

84: for (uint256 i = 0; i < _indices.length; i++) {
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

### G-07 Use `calldata` instead of `memory` for `function` parameters

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **5** instances of this issue:_

```solidity
File: /contracts/libraries/JBIpfsDecoder.sol

22: function decode(string memory _baseUri, bytes32 _hexString)

44: function _toBase58(bytes memory _source) private pure returns (string memory) {

66: function _truncate(uint8[] memory _array, uint8 _length) private pure returns (uint8[] memory) {

74: function _reverse(uint8[] memory _input) private pure returns (uint8[] memory) {

82: function _toAlphabet(uint8[] memory _indices) private pure returns (bytes memory) {
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol

### G-08 Not `using` the named `return` variables when a `function` `returns` wastes deployment gas

_There are **5** instances of this issue:_

```solidity
File: /contracts/JBTiered721DelegateProjectDeployer.sol

119: returns (uint256 configuration)

162: returns (uint256 configuration)
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol

```solidity
File: /contracts/JBTiered721Delegate.sol

123: function balanceOf(address _owner) public view override returns (uint256 balance) {

617: ) internal returns (uint256 leftoverAmount) {
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```solidity
File: /contracts/abstract/JB721Delegate.sol

105: function redeemParams(JBRedeemParamsData calldata _data)
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol
