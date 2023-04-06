# Asymmetry QA Report

## [L-01] Wrong events logic

### Impact

Functions `setPauseStaking` and `setPauseUnstaking` can be called with `_pause` equal to false but they would still call events named paused. This confusion will probably break the front-end or confuse users interacting directly with the contract.

### Proof Of Concept

```solidity
function setPauseStaking(bool _pause) external onlyOwner {
    pauseStaking = _pause;
    emit StakingPaused(pauseStaking);
}

function setPauseUnstaking(bool _pause) external onlyOwner {
    pauseUnstaking = _pause;
    emit UnstakingPaused(pauseUnstaking);
}
```

### Recommended

Change the naming or add separate functions for pausing and unpausing the staking.

## [L-02] Return values of `approve()` not checked

Not all IERC20 implementations `revert()` when there’s a failure in `approve()`. The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything.

```solidity
59: IERC20(STETH_TOKEN).approve(LIDO_CRV_POOL, stEthBal);
```

https://github.com/code-423n4/2023-03-asymmetry/blob/main/contracts/SafEth/derivatives/WstEth.sol
