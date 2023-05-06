# Biconomy

## Gas Optimizations

### G-01 Expressions for constant values such as a call to keccak256(), should use immutable rather than constant

It is expected that the value should be converted into a constant value at compile time. But actually the expression is re-calculated each time the constant is referenced.

```solidity
File: /smart-contract-wallet/SmartAccount.sol

136: return keccak256(abi.encode(DOMAIN_SEPARATOR_TYPEHASH, getChainId(), this));

217: txHash = keccak256(txHashData);

347: _signer = ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", dataHash)), v - 4, r, s);

416: return keccak256(enc

430: keccak256(

435: keccak256(_tx.data),
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/SmartAccount.sol

```solidity
File: /smart-contract-wallet/SmartAccountFactory.sol

34: bytes32 salt = keccak256(abi.encodePacked(_owner, address(uint160(_index))));

70: bytes32 salt = keccak256(abi.encodePacked(_owner, address(uint160(_index))));

71: bytes32 hash = keccak256(abi.encodePacked(bytes1(0xff), address(this), salt, keccak256
(code)));
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/SmartAccountFactory.sol

```solidity
File: /smart-contract-wallet/Proxy.sol

16: assert(_IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("biconomy.scw.proxy.implementation")) - 1));
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/Proxy.sol

```solidity
File: /smart-contract-wallet/SmartAccountNoAuth.sol

136: return keccak256(abi.encode(DOMAIN_SEPARATOR_TYPEHASH, getChainId(), this));

217: txHash = keccak256(txHashData);

342: _signer = ecrecover(keccak256(abi.encodePacked("\x19Ethereum Signed Message:\n32", dataHash)), v - 4, r, s);

406: return keccak256(encodeTransactionData(_tx, refundInfo, _nonce));

420: keccak256(

425: keccak256(_tx.data),
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/SmartAccountNoAuth.sol

```solidity
File: /smart-contract-wallet/aa-4337/core/EntryPoint.sol

197: return keccak256(abi.encode(userOp.hash(), address(this), block.chainid));
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/aa-4337/core/EntryPoint.sol

```solidity
File: /smart-contract-wallet/aa-4337/interfaces/UserOperation.sol

74: return keccak256(pack(userOp));
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/aa-4337/interfaces/UserOperation.sol

```solidity
File: /smart-contract-wallet/aa-4337/samples/SimpleAccountFactory.sol

44: return Create2.computeAddress(bytes32(salt), keccak256(abi.encodePacked(
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/aa-4337/samples/SimpleAccountFactory.sol

```solidity
File: /smart-contract-wallet/aa-4337/samples/TestAggregatedAccountFactory.sol

43: return Create2.computeAddress(bytes32(salt), keccak256(abi.encodePacked(
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/aa-4337/samples/TestAggregatedAccountFactory.sol

```solidity
File: /smart-contract-wallet/aa-4337/samples/VerifyingPaymaster.sol

39: return keccak256(abi.encode

42: keccak256(userOp.initCode),

43: keccak256(userOp.callData),
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/aa-4337/samples/VerifyingPaymaster.sol

```solidity
File: /smart-contract-wallet/common/Singleton.sol

13: assert(_IMPLEMENTATION_SLOT == bytes32(uint256(keccak256("biconomy.scw.proxy.implementation")) - 1));
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/common/Singleton.sol

```solidity
File: /smart-contract-wallet/paymasters/verifying/singleton/VerifyingSingletonPaymaster.sol

80: return keccak256(abi.encode(

83: keccak256(userOp.initCode),

84: keccak256(userOp.callData),
```

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/paymasters/verifying/singleton/VerifyingSingletonPaymaster.sol

### G-02 Long Revert Strings

Shortening revert strings to fit in 32 bytes will decrease gas costs for deployment and gas costs when the revert condition has been met.

https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/aa-4337/`*`
https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/common/Singleton.sol
https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/handler/DefaultCallbackHandler
https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/interfaces/ERC1155TokenReceiver
https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/interfaces/ERC721TokenReceiver.sol
https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/libs\MultiSend.sol
https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/paymasters/`*`
https://github.com/code-423n4/2023-01-biconomy/blob/main/scw-contracts/contracts/smart-contract-wallet/test/`*`
