# Juicebox

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **1** instances of this issue:_

```solidity
File: /contracts/JBTiered721Delegate.sol

677: _mint(_beneficiary, _tokenId);
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

### L-02 Use of floating `pragma`

_There are **10** instances of this issue:_

```solidity
File: /contracts/JB721GlobalGovernance.sol

File: /contracts/JBTiered721DelegateDeployer.sol

File: /contracts/JBTiered721DelegateProjectDeployer.sol

File: /contracts/JB721TieredGovernance.sol

File: /contracts/JBTiered721Delegate.sol

File: /contracts/JBTiered721DelegateStore.sol

File: /contracts/abstract/JB721Delegate.sol

File: /contracts/libraries/JBTiered721FundingCycleMetadataResolver.sol

File: /contracts/libraries/JBBitmap.sol

File: /contracts/libraries/JBIpfsDecoder.sol
```

### L-03 Zero-`address` checks are missing

_There are **1** instances of this issue:_

```solidity
File: /contracts/JBTiered721DelegateProjectDeployer.sol

91: _launchProjectFor(_owner, _launchProjectData);
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol

### N-01 Not using the named `return` variables anywhere in the `function` is confusing

_There are **5** instances of this issue:_

```solidity
File: /contracts/JBTiered721DelegateProjectDeployer.sol

119: returns (uint256 configuration)

162: returns (uint256 configuration)
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721DelegateProjectDeployer.sol

```solidity
File: /contracts/JBTiered721Delegate.sol

123: function balanceOf(address _owner) public view override returns (uint256 balance) {

617: ) internal returns (uint256 leftoverAmount) {
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

```solidity
File: /contracts/abstract/JB721Delegate.sol

105: function redeemParams(JBRedeemParamsData calldata _data)
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/abstract/JB721Delegate.sol

### N-02 `require()` / `revert()` statements should have descriptive reason strings

_There are **2** instances of this issue:_

```solidity
File: /contracts/JBTiered721Delegate.sol

216: require(address(this) != codeOrigin);

218: require(address(store) == address(0));
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/JBTiered721Delegate.sol

### N-03 `constants` should be defined rather than using magic numbers

_There are **9** instances of this issue:_

```solidity
File: /contracts/libraries/JBBitmap.sol

30: return (self.currentWord >> (_index % 256)) & 1 == 1;

52: self[_depth] |= uint256(1 << (_index % 256));

74: return _index / 256;
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBBitmap.sol

```solidity
File: /contracts/libraries/JBIpfsDecoder.sol

46: uint8[] memory digits = new uint8[](46); // hash size with the prefix

52: carry += uint256(digits[j]) * 256;

53: digits[j] = uint8(carry % 58);

54: carry = carry / 58;

58: digits[digitlength] = uint8(carry % 58);

60: carry = carry / 58;
```

https://github.com/jbx-protocol/juice-nft-rewards/blob/f9893b1497098241dd3a664956d8016ff0d0efd0/contracts/libraries/JBIpfsDecoder.sol
