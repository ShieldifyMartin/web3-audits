# [M-03] Front-running possibility

https://github.com/code-423n4/2023-01-drips/blob/main/src/NFTDriver.sol#L249
https://github.com/code-423n4/2023-01-drips/blob/main/src/NFTDriver.sol#L272
https://github.com/code-423n4/2023-01-drips/blob/main/src/AddressDriver.sol#L170

## Impact

The methods `approve`, `setApprovalForAll` and `_transferFromCaller` in `AddressDriver` and `NFTDriver` can be monitored for transactions and front-ran. Imagine the following scenario:

1. Bob holds some ERC20 tokens
2. For some reason, NFTDriver decides to approve Bob to spend specific amount of tokens.
3. Bob was expecting that and was already monitoring the mempool, so he front-runs the transaction with a transfer transaction to another address he controls
4. Now his is able to spend more thant expected.

## Proof of Concept

```solidity
249: function approve(address to, uint256 tokenId) public override whenNotPaused {

289: erc20.safeApprove(address(dripsHub), type(uint256).max);
```

```solidity
174: erc20.safeApprove(address(dripsHub), type(uint256).max);
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Use increase/decrease allowance instead.
