# [M-01] Front-running risk in key admin actions

## Impact

Let’s say Alice had approval of 100. Now the treasury custodian reduced the approval to 50. Alice could frontrun the setApprovalFor of 50, and withdraw 100 as it was before. Then withdraw 50 with the newly set approval. So the alice could withdraw 150.
The method can be monitored for transactions and front-ran. Imagine the following scenario:

1. Bob holds some VToken tokens
2. For some reason, Alice decides Bob is malicious and his approval should be zero, so he calls VToken::approve
3. Bob was expecting that and was already monitoring the mempool, so he front-runs the transaction with a transfer transaction to another address he controls
4. Now his approval is less, but he can still operate with his tokens from the new address.

`RiskFund` uses OpenZeppelin’s `safeApprove()` which has been documented as

1. Deprecated because of approve-like race condition
2. To be used only for initial setting of allowance (current allowance == 0) or resetting to 0 because it reverts otherwise.

## Proof of Concept

```solidity
133: function approve(address spender, uint256 amount) external override returns (bool) {
134:    require(spender != address(0), "invalid spender address");
135:    address src = msg.sender;
136:    transferAllowances[src][spender] = amount;
137:    emit Approval(src, spender, amount);
138:    return true;
139:}
```

```solidity
258: IERC20Upgradeable(underlyingAsset).safeApprove(pancakeSwapRouter, 0);

259: IERC20Upgradeable(underlyingAsset).safeApprove(pancakeSwapRouter, balanceOfUnderlyingAsset);
```

https://github.com/code-423n4/2023-05-venus/blob/main/contracts/RiskFund/RiskFund.sol

```solidity
322: token.safeApprove(address(vToken), 0);
323: token.safeApprove(address(vToken), amountToSupply);
```

https://github.com/code-423n4/2023-05-venus/blob/main/contracts/Pool/PoolRegistry.sol

## Tools Used

Manual Review

## Recommended Mitigation Steps

Remove the `approve` and `safeApprove` functions and use `safeIncreaseAllowance` and `safeDecreaseAllowance` instead.
