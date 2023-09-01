# [L-01] Missing `nonReentrant` modifier

In other functions that perform a call to send value `nonReentrant` is added but it's missing for `claim` function in `OperatorRewardsCollector` and `distributeRewards` and `settleFunds` in `ValidatorWithdrawalVault`.
