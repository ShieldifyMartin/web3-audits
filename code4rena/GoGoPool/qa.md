# Go Go Pool

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **4** instances of this issue:_

```solidity
File: /contract/tokens/TokenGGP.sol

13: _mint(msg.sender, TOTAL_SUPPLY);
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/TokenGGP.sol

```solidity
File: /contract/tokens/upgradeable/ERC4626Upgradeable.sol

49: _mint(receiver, shares);

62: _mint(receiver, shares);
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/upgradeable/ERC4626Upgradeable.sol

```solidity
File: /contract/tokens/TokenggAVAX.sol

176: _mint(msg.sender, shares);
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/TokenggAVAX.sol

### L-02 `emit` function called early

_There are **14** instances of this issue:_

```solidity
File: /contract/RewardsPool.sol

94: emit GGPInflated(newTokens);

161: emit NewRewardsCycleStarted(getRewardsCycleTotalAmt());

182: emit ProtocolDAORewardsTransfered(daoClaimContractAllotment);

187: emit MultisigRewardsTransfered(multisigClaimContractAllotment);

192: emit ClaimNodeOpRewardsTransfered(nopClaimContractAllotment);
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/RewardsPool.sol

```solidity
File: /contract/Storage.sol

36: emit GuardianChanged(address(0), msg.sender);
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/Storage.sol

```solidity
File: /contract/Vault.sol

54: emit AVAXDeposited(contractName, msg.value);

69: emit AVAXWithdrawn(contractName, amount);

93: emit AVAXTransfer(fromContractName, toContractName, amount);

126: emit TokenDeposited(contractKey, address(tokenContract), amount);

181: emit TokenTransfer(contractKeyFrom, contractKeyTo, address(tokenAddress), amount);
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/Vault.sol

```solidity
File: /contract/tokens/upgradeable/ERC4626Upgradeable.sol

109: emit Withdraw(msg.sender, receiver, owner, assets, shares);
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/upgradeable/ERC4626Upgradeable.sol

```solidity
File: /contract/tokens/TokenggAVAX.sol

185: emit Withdraw(msg.sender, msg.sender, msg.sender, assets, shares);

199: emit Withdraw(msg.sender, msg.sender, msg.sender, assets, shares);
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/TokenggAVAX.sol

### L-03 `if` check and `revert()` should be used instead of `assert()`

_There are **1** instances of this issue:_

```solidity
File: /contract/tokens/TokenggAVAX.sol

83: assert(msg.sender == address(asset));
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/TokenggAVAX.sol

### N-01 Open TODOs

_There are **2** instances of this issue:_

```solidity
File: /contracts/contract/BaseAbstract.sol

6: // TODO remove this when dev is complete
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/BaseAbstract.sol

```solidity
File: /contract/Staking.sol

203: // TODO cant use onlySpecificRegisteredContract("ClaimNodeOp", msg.sender) since we also call from increaseMinipoolCount. Wat do?
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/Staking.sol

### N-02 Unused `import`

_There are **5** instances of this issue:_

```solidity
File: /contracts/contract/ClaimNodeOp.sol

5: import {MinipoolManager} from "./MinipoolManager.sol";
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/ClaimNodeOp.sol

```solidity
File: /contracts/contract/MinipoolManager.sol

13: import {TokenGGP} from "./tokens/TokenGGP.sol";
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/MinipoolManager.sol

```soldity
File: /contract/ProtocolDAO.sol

5: import {TokenGGP} from "./tokens/TokenGGP.sol";
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/ProtocolDAO.sol

```solidity
File: /contract/Vault.sol

7: import {TokenGGP} from "./tokens/TokenGGP.sol";
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/Vault.sol

```solidity
File: /contract/tokens/TokenggAVAX.sol

7: import {ERC20Upgradeable} from "./upgradeable/ERC20Upgradeable.sol";
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/TokenggAVAX.sol

### N-03 Typo

_There are **3** instances of this issue:_

```solidity
File: /contract/Vault.sol

68: // Emit the withdrawl event for that contract

156: // Get the toke ERC20 instance

158: // Withdraw to the withdrawal address, , safeTransfer will revert if it fails
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/Vault.sol

### N-04 Non-floating `pragma` should be used

_There are **2** instances of this issue:_

```solidity
File: /contract/tokens/upgradeable/ERC4626Upgradeable.sol

File: /contract/tokens/upgradeable/ERC20Upgradeable.sol
```
