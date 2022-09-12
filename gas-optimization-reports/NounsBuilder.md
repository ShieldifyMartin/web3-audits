#  Nouns Builder
## Gas Optimizations Report
​
###  Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1` 
In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas
​
_There are **2** instances of this issue:_
​
```solidity
File: auction/Auction.sol
​
98: if (block.timestamp >= _auction.endTime) revert AUCTION_OVER();
```
​
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/auction/Auction.sol

```solidity
File: governance/treasury/Treasury.sol
​
89: return timestamps[_proposalId] != 0 && block.timestamp >= timestamps[_proposalId];
```
​
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/governance/treasury/Treasury.sol

### `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **6** instances of this issue:_

```solidity
File: governance/governor/Governor.sol

280: proposal.againstVotes += uint32(weight);

285: proposal.forVotes += uint32(weight);

290: proposal.abstainVotes += uint32(weight);
```
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/governance/governor/Governor.sol

```solidity
File: token/metadata/MetadataRenderer.sol

140: _propertyId += numStoredProperties;
```
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/token/metadata/MetadataRenderer.sol

```solidity
File: token/Token.sol

88: if ((totalOwnership += uint8(founderPct)) > 100) revert INVALID_FOUNDER_OWNERSHIP();

118: (baseTokenId += schedule) % 100;
```
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/token/Token.sol

### Use `calldata` instead of `memory` for function parameters
If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **9** instances of this issue:_

```solidity
File: governance/governor/Governor.sol

99: address[] memory _targets,

100: uint256[] memory _values,

101: bytes[] memory _calldatas,

117: address[] memory _targets,

118: uint256[] memory _values,

119: bytes[] memory _calldatas,

120: string memory _description

195: string memory _reason

252: string memory _reason
```
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/governance/governor/Governor.sol

### It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied
Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

_There are **5** instances of this issue:_

```solidity
File: governance/treasury/Treasury.sol

162: for (uint256 i = 0; i < numTargets; ++i) {
```

https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/governance/treasury/Treasury.sol

```solidity
File: token/metadata/MetadataRenderer.sol

119: for (uint256 i = 0; i < numNewProperties; ++i) {

133: for (uint256 i = 0; i < numNewItems; ++i) {

189: for (uint256 i = 0; i < numProperties; ++i) {

229: for (uint256 i = 0; i < numProperties; ++i) {
```

https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/token/metadata/MetadataRenderer.sol