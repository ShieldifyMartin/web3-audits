# Go Go Pool

## Gas Optmizations Report

### G-01 Splitting `require()` statements that use `&&` saves gas

Instead of using `&&` on single require check using two require checks can save gas

_There are **1** instances of this issue:_

```solidity
File: /contract/tokens/upgradeable/ERC20Upgradeable.sol

154: require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/upgradeable/ERC20Upgradeable.sol

### G-02 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **7** instances of this issue:_

```solidity
File: /contract/tokens/upgradeable/ERC20Upgradeable.sol

184: totalSupply += amount;

189: balanceOf[to] += amount;

196: balanceOf[from] -= amount;

201: totalSupply -= amount;
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/upgradeable/ERC20Upgradeable.sol

```solidity
File: /contract/tokens/TokenggAVAX.sol

160: stakingTotalAssets += assets;

245: totalReleasedAssets -= amount;

252: totalReleasedAssets += amount;
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/TokenggAVAX.sol

### G-03 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y – 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **1** instances of this issue:_

```solidity
File: /contract/tokens/upgradeable/ERC20Upgradeable.sol

127: require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");
```

https://github.com/code-423n4/2022-12-gogopool/blob/main/contracts/contract/tokens/upgradeable/ERC20Upgradeable.sol
