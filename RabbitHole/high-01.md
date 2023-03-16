# [H-01] Everyone can withdraw any ERC20 Quest remaining rewards

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/Quest.sol#L76
https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/Erc20Quest.sol#L102

## Impact

`onlyAdminWithdrawAfterEnd` modifier doesn't check if `msg.sender` is the owner, but the name ensures that it is checked. It results in every user is able to execute `withdrawFee` fucntion in `Erc20Quest` until the Quest has ended. Therefore performing unauthorized actions is high severity issue.

## Proof of Concept

```solidity
modifier onlyAdminWithdrawAfterEnd() {
```

```solidity
function withdrawFee() public onlyAdminWithdrawAfterEnd {
    IERC20(rewardToken).safeTransfer(protocolFeeRecipient, protocolFee());
}
```

## Tools Used

Manual audit

## Recommended Mitigation Steps

Perform onlyOwner check in the `onlyAdminWithdrawAfterEnd`.
