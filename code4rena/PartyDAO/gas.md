# PartyDAO
## Gas Optimisation Report

### G-01 The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

_There are **1** instances of this issue:_

```solidity
File: contracts/crowdfund/CollectionBuyCrowdfund.sol

62: for (uint256 i; i < hosts.length; i++) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CollectionBuyCrowdfund.sol

### G-02 Not using the named return variables when a function returns wastes deployment gas

_There are **5** instances of this issue:_

```solidity
File: contracts/party/PartyGovernanceNFT.sol

70: returns (address owner)
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernanceNFT.sol

```solidity
File: contracts/proposals/ListOnZoraProposal.sol

82: returns (bytes memory nextProgressData)

172: returns (bool sold)
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

406: returns (bytes32 balanceId)
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

127: returns (bytes memory nextProgressData)
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

### G-03 It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied

_There are **10** instances of this issue:_

```solidity
File: contracts/proposals/ArbitraryCallsProposal.sol

52: for (uint256 i = 0; i < hadPreciouses.length; ++i) {

61: for (uint256 i = 0; i < calls.length; ++i) {

78: for (uint256 i = 0; i < hadPreciouses.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ArbitraryCallsProposal.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

230: for (uint256 i = 0; i < infos.length; ++i) {

239: for (uint256 i = 0; i < infos.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

291: for (uint256 i = 0; i < fees.length; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

180: for (uint256 i = 0; i < contributors.length; ++i) {

242: for (uint256 i = 0; i < numContributions; ++i) {

300: for (uint256 i = 0; i < preciousTokens.length; ++i) {

348: for (uint256 i = 0; i < numContributions; ++i) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

### G-04 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **8** instances of this issue:_

```solidity
File: contracts/distribution/TokenDistributor.sol

381: _storedBalances[balanceId] -= amount;
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

243: ethContributed += contributions[i].amount;

352: ethOwed += c.amount;

355: ethUsed += c.amount;

359: ethUsed += partialEthUsed;

374: votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up

411: totalContributions += amount;

427: lastContribution.amount += amount;
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

### G-05 Using `> 0` costs less gas than `!= 0` when used on a `uint`

_There are **2** instances of this issue:_

```solidity
File: contracts/crowdfund/Crowdfund.sol

144: if (initialBalance > 0) {

471: if (votingPower > 0) {
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol