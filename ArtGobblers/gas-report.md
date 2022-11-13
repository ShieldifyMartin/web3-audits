# Art Gobblers

## Gas Optimisation Report

### G-01 It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied

_There are **2** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

432: for (uint256 i = 0; i < cost; ++i) {

592: for (uint256 i = 0; i < numGobblers; ++i) {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

### G-02 Using private rather than public for constants, saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

_There are **8** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

112: uint256 public constant MAX_SUPPLY = 10000;

115: uint256 public constant MINTLIST_SUPPLY = 2000;

118: uint256 public constant LEGENDARY_SUPPLY = 10;

122: uint256 public constant RESERVED_SUPPLY = (MAX_SUPPLY - MINTLIST_SUPPLY - LEGENDARY_SUPPLY) / 5;

126: uint256 public constant MAX_MINTABLE = MAX_SUPPLY

177: uint256 public constant LEGENDARY_GOBBLER_INITIAL_START_PRICE = 69;

180: uint256 public constant FIRST_LEGENDARY_GOBBLER_ID = MAX_SUPPLY - LEGENDARY_SUPPLY + 1;

184: uint256 public constant LEGENDARY_AUCTION_INTERVAL = MAX_MINTABLE / (LEGENDARY_SUPPLY + 1);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

### G-03 Functions that are access-restricted from most users may be marked as `payable`

_There are **1** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

560: function upgradeRandProvider(RandProvider newRandProvider) external onlyOwner {
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

/In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas/

### G-04 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

_There are **5** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

435: if (id >= FIRST_LEGENDARY_GOBBLER_ID) revert CannotBurnLegendary(id);

462: cost <= LEGENDARY_GOBBLER_INITIAL_START_PRICE / 2 ? LEGENDARY_GOBBLER_INITIAL_START_PRICE : cost * 2

497: if (numMintedSinceStart >= LEGENDARY_AUCTION_INTERVAL) return 0;

695: if (gobblerId <= gobblerRevealsData.lastRevealedId) {

702: if (gobblerId <= currentNonLegendaryId) return UNREVEALED_URI;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

### G-05 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **9** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

439: burnedMultipleTotal += getGobblerData[id].emissionMultiple;

456: getUserData[msg.sender].emissionMultiple += uint32(burnedMultipleTotal); // Update multiple.

464: legendaryGobblerAuctionData.numSold += 1;

662: getUserData[currentIdOwner].emissionMultiple += uint32(newCurrentIdMultiple);

844: uint256 newNumMintedForReserves = numMintedForReserves += (numGobblersEach << 1);

905: getUserData[from].emissionMultiple -= emissionMultiple;

906: getUserData[from].gobblersOwned -= 1;

912: getUserData[to].emissionMultiple += emissionMultiple;

913: getUserData[to].gobblersOwned += 1;
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

### G-06 Use custom errors rather than revert()/require() strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **4** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

437: require(getGobblerData[id].owner == msg.sender, "WRONG_FROM");

885: require(from == getGobblerData[id].owner, "WRONG_FROM");

887: require(to != address(0), "INVALID_RECIPIENT");

889: require(
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol
