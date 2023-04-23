# Esher

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **5** instances of this issue:_

```solidity
File: /src/Escher.sol

63: _mint(_account, uint256(_role), 1, "0x0");
```

https://github.com/code-423n4/2022-12-escher/blob/main/src/Escher.sol

```solidity
File: /src/Escher721.sol

52: _mint(to, tokenId);
```

https://github.com/code-423n4/2022-12-escher/blob/main/src/Escher721.sol

```solidity
File: /src/minters/FixedPrice.sol

66: nft.mint(msg.sender, x);
```

https://github.com/code-423n4/2022-12-escher/blob/main/src/minters/FixedPrice.sol

```solidity
File: /src/minters/OpenEdition.sol

67: nft.mint(msg.sender, x);
```

https://github.com/code-423n4/2022-12-escher/blob/main/src/minters/OpenEdition.sol

```solidity
File: /src/minters/LPDA.sol

74: nft.mint(msg.sender, x);
```

https://github.com/code-423n4/2022-12-escher/blob/main/src/minters/LPDA.sol

### L-02 Avoid `self-destruct`

Using locker is a better alternative

_There are **2** instances of this issue:_

```solidity
File: /src/minters/FixedPrice.sol

110: selfdestruct(_sale.saleReceiver);
```

https://github.com/code-423n4/2022-12-escher/blob/main/src/minters/FixedPrice.sol

```solidity
File: /src/minters/OpenEdition.sol

122: selfdestruct(_sale.saleReceiver);
```

https://github.com/code-423n4/2022-12-escher/blob/main/src/minters/OpenEdition.sol

### L-03 `emit` function called early

_There are **1** instances of this issue:_

```solidity
File: /src/minters/FixedPrice.sol

108: emit End(_sale);
```

https://github.com/code-423n4/2022-12-escher/blob/main/src/minters/FixedPrice.sol

### L-04 Use `safeTransfer` instead of `transfer`

_There are **3** instances of this issue:_

```solidity
File: /src/minters/LPDA.sol

85: ISaleFactory(factory).feeReceiver().transfer(fee);

86: temp.saleReceiver.transfer(totalSale - fee);

105: payable(msg.sender).transfer(owed);
```

https://github.com/code-423n4/2022-12-escher/blob/main/src/minters/LPDA.sol

### N-01 Non-floating `pragma` should be used

_There are **12** instances of this issue:_

```solidity
File: src/uris/Unique.sol

File: src/uris/Base.sol

File: src/uris/Generative.sol

File: src/Escher721Factory.sol

File: src/minters/FixedPriceFactory.sol

File: src/minters/OpenEditionFactory.sol

File: src/minters/LPDAFactory.sol

File: src/Escher.sol

File: src/Escher721.sol

File: src/minters/FixedPrice.sol

File: src/minters/OpenEdition.sol

File: src/minters/LPDA.sol
```
