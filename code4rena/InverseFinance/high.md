https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol#L158

## Impact

Potentially drain more funds than allowed.

## Proof of Concept

Let’s say UserOne had approval of 100. Now the treasury custodian reduced the approval to 50. UserOne could frontrun the setApprovalFor of 50, and withdraw 100 as it was before. Then withdraw 50 with the newly set approval. So the UserOne could withdraw 150.

```solidity
function approve(address spender, uint256 amount) public virtual returns (bool) {
    allowance[msg.sender][spender] = amount;
    emit Approval(msg.sender, spender, amount);
    return true;
}
```

## Recommended Migration Steps

Instead of setting the given amount, one can reduce from the current approval. By doing so, it checks whether the previous approval is spend.
