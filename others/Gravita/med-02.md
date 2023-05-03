# [M-02] `unstake` function is not compatible with tokens that implement a block list feature

## Impact

Some tokens, for example USDC and USDT implement an admin controlled address block list. All transfers to a blocked address will revert. Since the `unstake` functionality transfers the claimable tokens to the `msg.sender` address, the call will revert if such an address has claimable balance and is in the token's block list.

## Code snippet

https://github.com/Gravita-Protocol/Gravita-SmartContracts/blob/main/contracts/GRVT/GRVTStaking.sol#L118

## Tools Used

Manual Review

## Recommended Mitigation Steps

Use the Pull over Push pattern to send tokens out of the contract in a revoke scenario.
