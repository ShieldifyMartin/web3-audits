# Introduction

`SureFX` security review was done by [martin](https://twitter.com/MartinPetrov033), with a main focus on the security aspects of the implementation and logic of the project.

# Project Overview

Surefx provides users with easy-to-use exchange rate hedging solutions.

# Additional information

Because there is still no stablecoin product for currencies like COP the sureFX team has forked the usdc contract (found in the repo under the FiatToken folder) to create a stablecoin for COP.

# Overall Health

The protocol is well-tested with role separation and high test coverage.

# Severity classification

| Severity level         | Impact: High | Impact: Medium | Impact: Low |
| ---------------------- | ------------ | -------------- | ----------- |
| **Likelihood: High**   | Critical     | High           | Medium      |
| **Likelihood: Medium** | High         | Medium         | Low         |
| **Likelihood: Low**    | Medium       | Low            | Low         |

# Security Assessment Summary

### Scope

The following smart contracts were in scope of the audit:

- [HedgeManager](https://gist.github.com/wesleyfsmith/f43c0ee699fdeb5aa38a0b54438d478b)

---

# Findings Summary

The following number of issues were found, categorized by their severity:

- <b>High</b>: 0 issues
- <b>Medium</b>: 2 issues
- <b>Low</b>: 4 issues
- <b>Informational</b>: 6 issues

| ID     | Title                                                                        | Severity      |
| ------ | ---------------------------------------------------------------------------- | ------------- |
| [M-01] | Centralisation Vulnerability                                                 | Medium        |
| [M-02] | Single Point of Failure                                                      | Medium        |
| [L-01] | Faulty check message                                                         | Low           |
| [L-02] | Repetitive logic should be extracted as a modifier or function               | Low           |
| [L-03] | Misplaced check                                                              | Low           |
| [L-04] | Misleading comments and function naming                                      | Low           |
| [I-01] | Missing NatSpec                                                              | Informational |
| [I-02] | Use a more recent version of solidity                                        | Informational |
| [I-03] | Missing `ganache-cli`                                                        | Informational |
| [I-04] | Use scientific notation (e.g. 1e6) rather than exponentiation (e.g. 10\*\*6) | Informational |
| [I-05] | Unused import                                                                | Informational |
| [I-06] | Better naming could be used                                                  | Informational |

# Detailed Findings

## [M-01] Centralisation Vulnerability

### Severity

**Likelihood:**
Medium, because these functions can not be called and funds are stuck when paused

**Impact:**
Medium, because it limits the functionality of the protocol

### Description

It's a centralisation vulnerability if an owner can pause not only inbound, but outbound functionality. It's a best practice to be able to pause inbound (`liquidateHedge` and `addCollateral`) functionality but not outbound (`liquidateHedge` and `closeHedge`) methods.

### Proof of Concept

```solidity
117: function liquidateHedge(uint256 _hedgeId) external whenNotPaused onlyRole(LIQUIDATOR) {

217: function closeHedge(uint256 _hedgeId) external whenNotPaused {
```

### Recommendations

Remove `whenNotPaused` modifier from `liquidateHedge` and `closeHedge` functions.

## [M-02] Single Point of Failure

### Severity

**Likelihood:**
Very Low, because it requires malicious admin or stolen admin private key

**Impact:**
Critical, because it would drain all available funds

### Description

The `DEFAULT_ADMIN_ROLE` has a single point of failure and only the admin can use `withdrawCopcPANIC` and `withdrawUsdcPANIC` critical functions. Admin is not behind a multi-sig and changes are not behind a timelock. Even if protocol admin is not malicious there is still a chance for the keys to be stolen. In such a case, the attacker can cause serious damage to the project due to these important functions. In such a case, users who have invested in the project will suffer high financial losses.

### Proof of Concept

https://gist.github.com/wesleyfsmith/f43c0ee699fdeb5aa38a0b54438d478b#file-hedgemanger-sol-L394-L406

### Recommendations

Check if an address is a contract, you can use the extcodesize opcode, which returns the size of the code at a given address. If the code size is greater than zero, the address is a contract.

## [L-01] Faulty check message

Change the `require` message to "HedgeClosedOrLiquidated".

https://gist.github.com/wesleyfsmith/f43c0ee699fdeb5aa38a0b54438d478b#file-hedgemanger-sol-L197-L201

https://gist.github.com/wesleyfsmith/f43c0ee699fdeb5aa38a0b54438d478b#file-hedgemanger-sol-L229-L233

## [L-02] Repetitive logic should be extracted as a modifier or function

The modifier could be called `isAdminOrHedgeOwner`.

https://gist.github.com/wesleyfsmith/f43c0ee699fdeb5aa38a0b54438d478b#file-hedgemanger-sol-L191-L195

## [L-03] Misplaced check

The intention is to ensure that `currentRate` is not equal to zero, the require statement should come before the assignment. This will first check if `currentRate` is non-zero, and only assign a new value if the check passes. If `currentRate` is equal to zero, the transaction will revert with "ExchangeRateZero".

https://gist.github.com/wesleyfsmith/f43c0ee699fdeb5aa38a0b54438d478b#file-hedgemanger-sol-L427-L428

## [L-04] Misleading comments and function naming

Mention that the freeze logic is only for hedge functionality. Otherwise there are high impact vulnerabilities.

https://gist.github.com/wesleyfsmith/f43c0ee699fdeb5aa38a0b54438d478b#file-hedgemanger-sol-L527

Misleading comment - should be `add collateral` instead of `close position`

https://gist.github.com/wesleyfsmith/f43c0ee699fdeb5aa38a0b54438d478b#file-hedgemanger-sol-L189

## [I-01] Missing NatSpec

Following [the Solidity Style Guide sections](https://docs.soliditylang.org/en/v0.8.18/style-guide.html), try to write as detailed comments as possible to make it easier for auditors and developers to understand the purpose of each particular feature and its parameters.

## [I-02] Use a more recent version of solidity

Use latest Solidity version to get all compiler features, bugfixes and optimizations.

## [I-03] Missing `ganache-cli`

Add `ganache-cli` package to the project in order to be able to run the tests.

## [I-04] Use scientific notation (e.g. 1e6) rather than exponentiation (e.g. 10\*\*6)

## [I-05] Unused import

`hardhat/console.sol` import should be removed.

## [I-06] Better naming could be used

what -> contractType

https://gist.github.com/wesleyfsmith/f43c0ee699fdeb5aa38a0b54438d478b#file-hedgemanger-sol-L500

what -> property / param

https://gist.github.com/wesleyfsmith/f43c0ee699fdeb5aa38a0b54438d478b#file-hedgemanger-sol-L510

# Disclaimer

Smart contract reviews can never 100% guarantee the absence of vulnerabilities in the project. The author of the report takes no responsibility for damage arising in connection with this report.
