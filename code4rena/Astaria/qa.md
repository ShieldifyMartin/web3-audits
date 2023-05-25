# Astaria

## QA Report

### L-01 Hardcoded function

#### Impact

Returns wrong data when is not approved for every user. Could have security implication and I think low severity matches well.

```solidity
106: function isApprovedForAll(address account, address operator)
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/ClearingHouse.sol

## Recommended Mitigation Steps

Add function implementation.

### L-02 `_safeMint()` should be used rather than `_mint()` wherever possible

`safeMint` does an unsafe external call to the recipient, so there is a reentrency attack vector. Make sure to add 'nonReentrant' modifier.

_There are **5** instances of this issue:_

```solidity
139: _mint(receiver, shares);
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/WithdrawProxy.sol

```solidity
588: _mint(from_, collateralId);
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/CollateralToken.sol

```solidity
512: _mint(msg.sender, unclaimed);
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/PublicVault.sol

```solidity
31: _mint(receiver, shares);

47: _mint(receiver, shares);
```

https://github.com/AstariaXYZ/astaria-gpl/blob/4b49fe993d9b807fe68b3421ee7f2fe91267c9ef/src/ERC4626-Cloned.sol

### N-01 Unused imports

_There are **10** instances of this issue:_

```solidity
17: import {WETH} from "solmate/tokens/WETH.sol";

24: import {
  ConduitControllerInterface
} from "seaport/interfaces/ConduitControllerInterface.sol";
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/ClearingHouse.sol

```solidity
33: import {VaultImplementation} from "core/VaultImplementation.sol";

53:  OrderComponents,

59: import {SeaportInterface} from "seaport/interfaces/SeaportInterface.sol";
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/CollateralToken.sol

```solidity
24: import {IERC4626} from "core/interfaces/IERC4626.sol";
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/PublicVault.sol

```solidity
34: import {VaultImplementation} from "./VaultImplementation.sol";
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/LienToken.sol

```solidity
18: import {IERC4626} from "core/interfaces/IERC4626.sol";

21: import {IRouterBase} from "core/interfaces/IRouterBase.sol";
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/AstariaVaultBase.sol

```solidity
25: import {IPublicVault} from "core/interfaces/IPublicVault.sol";
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/VaultImplementation.sol

### N-02 Use elementary types or a user-defined type instead of a `struct` that has only one member

```solidity
36: struct ClearingHouseStorage {
    ILienToken.AuctionData auctionStack;
  }
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/ClearingHouse.sol

### N-03 Using variable instead of `function`

```solidity
67: function minDepositAmount()
```

https://github.com/code-423n4/2023-01-astaria/blob/main/src/WithdrawProxy.sol

### N-04 Smart contract recommended structure is not followed

Functions, modifiers and events should not be mixed.

https://github.com/code-423n4/2023-01-astaria/blob/main/src/WithdrawProxy.sol

### N-05 Completely missing or absolutely insufficient NatSpec

https://github.com/code-423n4/2023-01-astaria/blob/main/src/TransferProxy.sol

https://github.com/code-423n4/2023-01-astaria/blob/main/src/Vault.sol

https://github.com/code-423n4/2023-01-astaria/blob/main/src/ClearingHouse.sol

https://github.com/code-423n4/2023-01-astaria/blob/main/src/WithdrawProxy.sol

https://github.com/code-423n4/2023-01-astaria/blob/main/src/CollateralToken.sol

https://github.com/code-423n4/2023-01-astaria/blob/main/src/PublicVault.sol

https://github.com/code-423n4/2023-01-astaria/blob/main/src/AstariaRouter.sol

https://github.com/code-423n4/2023-01-astaria/blob/main/src/LienToken.sol
