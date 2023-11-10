# PartyDAO
## QA Report

### L-01 Use of floating `pragma`

### L-02 `require()` should be used instead of `assert()`

_There are **8** instances of this issue:_

```solidity
File: contracts/crowdfund/CrowdfundNFT.sol

166: assert(false); // Will not be reached.
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/CrowdfundNFT.sol

```solidity
File: contracts/proposals/ListOnZoraProposal.sol

120: assert(step == ZoraStep.ListedOnZora);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol

```solidity
File: contracts/proposals/ProposalExecutionEngine.sol

142: assert(currentInProgressProposalId == 0);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol

```solidity
File: contracts/distribution/TokenDistributor.sol

385: assert(tokenType == TokenType.Erc20);

411: assert(tokenType == TokenType.Erc20);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

221: assert(step == ListOnOpenseaStep.ListedOnOpenSea);

302: assert(SEAPORT.validate(orders));
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

```solidity
File: contracts/party/PartyGovernance.sol

504: assert(tokenType == ITokenDistributor.TokenType.Erc20);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

### L-03 `emit` function called early

_There are **1** instances of this issue:_

```solidity
File: contracts/party/PartyGovernance.sol

497: emit DistributionCreated(tokenType, token, tokenId);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

### N-01 `require()`/`revert()` statements should have descriptive reason strings

_There are **2** instances of this issue:_

```solidity
File: contracts/proposals/ProposalExecutionEngine.sol

246: require(proposalType != ProposalType.Invalid);

247: require(uint8(proposalType) < uint8(ProposalType.NumProposalTypes));
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol

### N-02 `Event` is missing `indexed` fields
Each event should use three indexed fields if there are three or more fields

_There are **16** instances of this issue:_

```solidity
File: contracts/proposals/ListOnZoraProposal.sol

45: event ZoraAuctionCreated(

53: event ZoraAuctionExpired(uint256 auctionId, uint256 expiry);

54: event ZoraAuctionSold(uint256 auctionId);

55: event ZoraAuctionFailed(uint256 auctionId);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnZoraProposal.sol

```solidity
File: contracts/proposals/ProposalExecutionEngine.sol

66: event ProposalEngineImplementationUpgraded(address oldImpl, address newImpl);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ProposalExecutionEngine.sol

```solidity
File: contracts/crowdfund/BuyCrowdfundBase.sol

52: event Won(Party party, IERC721 token, uint256 tokenId, uint256 settledPrice);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/BuyCrowdfundBase.sol

```solidity
File: contracts/proposals/ListOnOpenseaProposal.sol

63: event OpenseaOrderListed(

71: event OpenseaOrderSold(

77: event OpenseaOrderExpired(
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/proposals/ListOnOpenseaProposal.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

82: event Burned(address contributor, uint256 ethUsed, uint256 ethOwed, uint256 votingPower);

83: event Contributed(address contributor, uint256 amount, address delegate, uint256 previousTotalContributions);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol

```solidity
File: contracts/party/PartyGovernance.sol

151: event Proposed(

156: event ProposalAccepted(

162: event PartyInitialized(GovernanceOpts opts, IERC721[] preciousTokens, uint256[] preciousTokenIds);

167: event DistributionCreated(ITokenDistributor.TokenType tokenType, address token, uint256 tokenId);

169: event HostStatusTransferred(address oldHost, address newHost);
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/party/PartyGovernance.sol

### N-03 Not using the named `return` variables anywhere in the `function` is confusing

_There are **3** instances of this issue:_

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

### N-04 `constants` should be defined rather than using magic numbers

_There are **7** instances of this issue:_

```solidity
File: contracts/distribution/TokenDistributor.sol

335: if (args.feeBps > 1e4) {

352: uint128 fee = supply * args.feeBps / 1e4;
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/distribution/TokenDistributor.sol

```solidity
File: contracts/crowdfund/Crowdfund.sol

129: if (opts.governanceOpts.feeBps > 1e4) {

132: if (opts.governanceOpts.passThresholdBps > 1e4) {

135: if (opts.splitBps > 1e4) {

370: votingPower = ((1e4 - splitBps_) * ethUsed) / 1e4;

374: votingPower += (splitBps_ * totalEthUsed + (1e4 - 1)) / 1e4; // round up
```
https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol