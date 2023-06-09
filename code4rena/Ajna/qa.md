## Introduction

Ajna QA report was done by martin and anonresercher, with a main focus on the low severity and non-critical security aspects of the implementaion and logic of the project.

## Findings Summary

The following issues were found, categorized by their severity:

## Findings Summary

| ID      | Title                                                     | Severity     |
| ------- | --------------------------------------------------------- | ------------ |
| [L-01]  | Hardcoded Ajna token address                              | Low          |
| [L-02]  | Function logic is misleadingly documented                 | Low          |
| [L-03]  | Redundant parameter `reason` in `IFunding.VoteCast` event | Low          |
| [NC-01] | `emit` function called early                              | Non-Critical |
| [NC-02] | Bad formatting                                            | Non-Critical |

## [L-01] Hardcoded Ajna token address

In case the addresses change due to reasons such as updating their versions in the future, addresses coded as constants cannot be updated. You can have a `immutable` variable containing the token address. And this variable can be set from a constructor.

So you can effectively pass the value from an environment variable to the contract constructor, to the contract storage.

```solidity
21: address public immutable ajnaTokenAddress = 0x9a96ec9B57Fb64FbC60B423d1f4da7691Bd35079;
```

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/base/Funding.sol

## [L-02] Function logic is misleadingly documented

The Nat Spec dev description assures that the function will iterate through maximum of 10 proposals, but actually it iterates through all of them in a nested loop.

```solidity
458: * @dev    Only iterates through a maximum of 10 proposals that made it through the screening round.
```

## [L-03] Redundant parameter `reason` in `IFunding.VoteCast` event

The `reason` parameter is set value as an empty string everywhere where event `IFunding.VoteCast` is emitted and should therefore be removed, because there is no use for it

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/interfaces/IFunding.sol#L69

```solidity
/ajna-grants/src/grants/interfaces/IFunding.sol

69: event VoteCast(address indexed voter, uint256 proposalId, uint8 support, uint256 weight, string reason);
```

There are 3 instances of set `reason` value as an empty string:

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/base/ExtraordinaryFunding.sol#L155

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/base/StandardFunding.sol#L688

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/base/StandardFunding.sol#L751

## [NC-01] `emit` function called early

The event is actually emitted before the action, event emits should be moved after the action. Otherwise, it might cause an incorrectly emitted event.

```solidity
59: emit ProposalExecuted(proposalId_);
```

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/base/Funding.sol

In case `safeTransfer` fails the `DelegateRewardClaimed` event would still be called.

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/base/StandardFunding.sol#L257

In case `safeTransferFrom` fails the `FundTreasury` event would still be called.

https://github.com/code-423n4/2023-05-ajna/blob/
main/ajna-grants/src/grants/GrantFund.sol#L64

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-core/src/RewardsManager.sol#L299

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-core/src/RewardsManager.sol#L585

## [NC-02] Bad formatting

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/base/Funding.sol

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/base/ExtraordinaryFunding.sol

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/base/ExtraordinaryFunding.sol

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/base/StandardFunding.sol

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-grants/src/grants/GrantFund.sol

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-core/src/PositionManager.sol

https://github.com/code-423n4/2023-05-ajna/blob/main/ajna-core/src/RewardsManager.sol
