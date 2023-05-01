# [L-01] No error message is returned

Consider adding a error description, in order to stick to the best practises

```solidity
76: require(_GRVTamount > 0);
```

https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/GRVT/GRVTStaking.sol

# [L-02] Non-Conforming ERC20 Tokens Not Recoverable

There are tokens that do not return a value on success, which will cause the call to revert, even when the transfer would have been successful. This means that those tokens will be stuck forever and not be recoverable.
Someone accidentally transfers USDT, one of the most commonly used ERC20 tokens, to the contract. Because USDT’s transfer does not return a boolean, it will not be possible to recover those tokens and they will be stuck forever.
It's recommended to use OpenZeppelin’s `safeTransferFrom` instead of `transferFrom`.

```solidity
111: grvtToken.transferFrom(msg.sender, address(this), _GRVTamount);
```

https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/GRVT/GRVTStaking.sol

# [L-03] Misleading name is used

`setActive` function name gives a wrong assumption that collateral can only be activated, which is totally incorrect.

```solidity
263: function setActive(address _collateral, bool _active) public onlyOwner {
```

https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/AdminContract.sol

# [L-04] `require()` should be used instead of `assert()`

```solidity
299: assert(
300:    msg.sender == _borrower || (address(stabilityPool) == msg.sender &&  _assetSent > 0 && _debtTokenChange == 0)
301:);
```

https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/BorrowerOperations.sol

# [NC-01] File is missing NatSpec

Following the Solidity Style Guide sections, try to write as detailed comments as possible to make it easier for auditors, developers or other technical guys to understand the purpose of each particular feature and its parameters.

https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/GRVT/CommunityIssuance.sol
https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/GRVT/GRVTStaking.sol
https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/GRVT/GRVTToken.sol
https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/GRVT/LockedGRVT.sol
https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/ActivePool.sol
https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/AdminContract.sol
https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/AdminContract.sol
https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/BorrowerOperations.sol

# [NC-02] Mixing alternative ways to perform checks is discouraged

In `safeCheck` modifier both `require` and custom error are used. Consider using a consistent way to perform checks. Using custom error is the better approach since it is the more often used one in the contract and has a large number of positives anyway.

https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/AdminContract.sol#L91
