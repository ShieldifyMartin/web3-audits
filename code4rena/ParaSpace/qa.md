# ParaSpace

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **2** instances of this issue:_

```solidity
File: /protocol/libraries/logic/SupplyLogic.sol

110: bool isFirstSupply = IPToken(reserveCache.xTokenAddress).mint(
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/SupplyLogic.sol

```solidity
File: /ui/WPunkGateway.sol

86: WPunk.mint(punkIndexes[i].tokenId);
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/ui/WPunkGateway.sol

### N-01 Unused imports

_There are **6** instances of this issue:_

```solidity
File: /misc/marketplaces/LooksRareAdapter.sol

11: import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

```solidity
File: /misc/marketplaces/SeaportAdapter.sol

10: import {ConsiderationItem} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";

11: import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/SeaportAdapter.sol

```solidity
File: /misc/marketplaces/X2Y2Adapter.sol

12: import {AdvancedOrder, CriteriaResolver, Fulfillment, OfferItem, ItemType} from "../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/X2Y2Adapter.sol

```solidity
File: /protocol/libraries/logic/MarketplaceLogic.sol

21: import {AdvancedOrder, CriteriaResolver, Fulfillment} from "../../../dependencies/seaport/contracts/lib/ConsiderationStructs.sol";
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

```solidity
File: /protocol/libraries/logic/PoolLogic.sol

16: import {IXTokGenericLogicenType, XTokenType} from "../../../interfaces/IXTokenType.sol";
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/PoolLogic.sol

### N-02 Open TODOs

_There are **3** instances of this issue:_

```solidity
File: /misc/marketplaces/LooksRareAdapter.sol

59: makerAsk.price, // TODO: take minPercentageToAsk into account
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/marketplaces/LooksRareAdapter.sol

```solidity
File: /misc/UniswapV3OracleWrapper.sol

218: revert("unimplemented");
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

```solidity
File: /protocol/libraries/logic/MarketplaceLogic.sol

442: // TODO: support PToken
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/protocol/libraries/logic/MarketplaceLogic.sol

### N-03 Not using the named `return` variables anywhere in the `function` is confusing

Consider changing the variable to be an unnamed one

_There are **2** instances of this issue:_

```solidity
File: /misc/NFTFloorOracle.sol

240: returns (uint256 price)

256: returns (uint256 timestamp)
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/NFTFloorOracle.sol

### N-04 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

_There are **4** instances of this issue:_

```solidity
File: /misc/UniswapV3OracleWrapper.sol

245: ((oracleData.token0Price * (10**18)) /

247: ) * 2**96) / 1E9

259: ) * 2**96) / 1E9

271: ) * 2**96) /
```

https://github.com/code-423n4/2022-11-paraspace/blob/main/paraspace-core/contracts/misc/UniswapV3OracleWrapper.sol

### N-05 Use a more recent version of solidity

_There are **32** instances of this issue:_

```solidity
File: /misc/marketplaces/LooksRareAdapter.sol

File: /misc/marketplaces/SeaportAdapter.sol

File: /misc/marketplaces/X2Y2Adapter.sol

File: /misc/NFTFloorOracle.sol

File: /misc/ParaSpaceOracle.sol

File: /misc/UniswapV3OracleWrapper.sol

File: protocol/configuration/PoolAddressesProvider.sol

File: protocol/libraries/logic/AuctionLogic.sol

File: /protocol/libraries/logic/BorrowLogic.sol

File: /protocol/libraries/logic/FlashClaimLogic.sol

File: protocol/libraries/logic/GenericLogic.sol

File: /protocol/libraries/logic/LiquidationLogic.sol

File: /protocol/libraries/logic/MarketplaceLogic.sol

File: /protocol/libraries/logic/PoolLogic.sol

File: /protocol/libraries/logic/SupplyLogic.sol

File: /protocol/libraries/logic/ValidationLogic.sol

File: /protocol/pool/DefaultReserveAuctionStrategy.sol

File: /protocol/pool/PoolApeStaking.sol

File: /protocol/pool/PoolCore.sol

File: /protocol/pool/PoolMarketplace.sol

File: /protocol/pool/PoolParameters.sol

File: /protocol/pool/PoolStorage.sol

File: /protocol/tokenization/base/MintableIncentivizedERC721.sol

File: /protocol/tokenization/NToken.sol

File: /protocol/tokenization/NTokenApeStaking.sol

File: /protocol/tokenization/NTokenBAYC.sol

File: /protocol/tokenization/NTokenMAYC.sol

File: /protocol/tokenization/NTokenMoonBirds.sol

File: /protocol/tokenization/NTokenUniswapV3.sol

File: /protocol/tokenization/libraries/MintableERC721Logic.sol

File: /protocol/tokenization/libraries/ApeStakingLogic.sol

File: /ui/WPunkGateway.sol
```
