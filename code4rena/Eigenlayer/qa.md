## Introduction

EigenLayer QA report was done by martin, with a main focus on the low severity and non-critical security aspects of the implementaion and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

# Findings Summary

| ID      | Title                                                              | Severity     |
| ------- | ------------------------------------------------------------------ | ------------ |
| [L-01]  | Useless `withdraw` function pausing functionality                  | Low          |
| [L-02]  | `emit` function called early                                       | Low          |
| [L-03]  | Flawed logic and inconsitency for important state-changing methods | Low          |
| [NC-01] | File formatting issues                                             | Non-Critical |

# [L-01] Useless `withdraw` function pausing functionality

In general it's a centralisation vulnerability if an owner can pause not only inbound, but also outbound functionality. But in this case `withdraw` function can be called by the `strategyManager` anyway, so the pausing functionality is absolutely redudant.

https://github.com/code-423n4/2023-04-eigenlayer/blob/main/src/contracts/strategies/StrategyBase.sol#L121

https://github.com/code-423n4/2023-04-eigenlayer/blob/main/src/contracts/pods/EigenPodManager.sol#L150

## [L-02] `emit` function called early

The event is actually emitted before the action, event emits should be moved after the action. Otherwise, it might cause an incorrectly emitted event.

https://github.com/code-423n4/2023-04-eigenlayer/blob/main/src/contracts/permissions/PauserRegistry.sol#L43

https://github.com/code-423n4/2023-04-eigenlayer/blob/main/src/contracts/permissions/PauserRegistry.sol#L49

https://github.com/code-423n4/2023-04-eigenlayer/blob/main/src/contracts/pods/DelayedWithdrawalRouter.sol#L168

https://github.com/code-423n4/2023-04-eigenlayer/blob/main/src/contracts/core/StrategyManager.sol#L841

https://github.com/code-423n4/2023-04-eigenlayer/blob/main/src/contracts/core/StrategyManager.sol#L847

## [L-03] Flawed logic and inconsitency for important state-changing methods

There is a function to `pauseAll` that can be called only by `pauser` which sets `_paused` to 2^256-1. However in the `whenNotPaused` modifier it only checks if it is 0 or not. Consider functionality redesign.
In addition there isn't a `unpauseAll` function that can be called only by `unpauser`.

```solidity
function pauseAll() external onlyPauser {
```

https://github.com/code-423n4/2023-04-eigenlayer/blob/main/src/contracts/permissions/Pausable.sol

## [NC-01] File formatting issues

Use a code formatter to keep code clean & tidy, consider adding the `forge fmt` command pre-commit hook.

https://github.com/code-423n4/2023-04-eigenlayer/blob/main/src/contracts/libraries/BeaconChainProofs.sol
