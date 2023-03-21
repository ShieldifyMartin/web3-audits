# Art Gobblers

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **3** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

356: _mint(msg.sender, gobblerId);

389: _mint(msg.sender, gobblerId);

469: _mint(msg.sender, gobblerId);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol

### N-01 Event is missing indexed fields

Each event should use three indexed fields if there are three or more fields

_There are **1** instances of this issue:_

```solidity
File: src/ArtGobblers.sol

236: event RandomnessFulfilled(uint256 randomness);
```

https://github.com/code-423n4/2022-09-artgobblers/blob/main/src/ArtGobblers.sol
