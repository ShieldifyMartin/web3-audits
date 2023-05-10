# Biconomy

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

```solidity
12: _mint(sender, amount);
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/test/MockToken.sol

```solidity
17: _mint(sender, amount);

22: _mint(_for, amount);
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/test/StakedTestToken.sol

### L-02 `require()` should be used instead of `assert()`

```solidity
16: assert(_IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("biconomy.scw.proxy.implementation")) - 1));
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/Proxy.sol

### L-03 Unused `receive()`/`fallback()` function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds

```solidity
550: receive() external payable {}
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/SmartAccount.sol

```solidity
540: receive() external payable {}
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/SmartAccountNoAuth.sol

### L-04 Approve return values aren't checked

```solidity
17: token.approve(paymaster, type(uint256).max);
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/aa-4337/samples/SimpleAccountForTokens.sol

### N-01 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/libs/Math.sol

### N-02 Non-floating `pragma` should be used

https://github.com/code-423n4/2023-01-biconomy/tree/main/scw-contracts/contracts/smart-contract-wallet/aa-4337/*

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/paymasters/BasePaymaster.sol
