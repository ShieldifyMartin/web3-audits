#  Nouns Builder
## QA Report

### L-01 `emit` function called early
​
_There are **3** instances of this issue:_
​
```solidity
File: token/metadata/MetadataRenderer.sol
​
348: emit ContractImageUpdated(settings.contractImage, _newContractImage);

356: emit RendererBaseUpdated(settings.rendererBase, _newRendererBase);

364: emit DescriptionUpdated(settings.description, _newDescription);
```
​
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/token/metadata/MetadataRenderer.sol

### N-01 Event is missing `indexed` fields
​
_There are **32** instances of this issue:_
​
```solidity
File: auction/IAuction.sol
​
22: event AuctionBid(uint256 tokenId, address bidder, uint256 amount, bool extended, uint256 endTime);

28: event AuctionSettled(uint256 tokenId, address winner, uint256 amount);

34: event AuctionCreated(uint256 tokenId, uint256 startTime, uint256 endTime);

38: event DurationUpdated(uint256 duration);

42: event ReservePriceUpdated(uint256 reservePrice);

46: event MinBidIncrementPercentageUpdated(uint256 minBidIncrementPercentage);

50: event TimeBufferUpdated(uint256 timeBuffer);
```
​
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/auction/IAuction.sol

```solidity
File: governance/governor/IGovernor.sol

18: event ProposalCreated(​

29: event ProposalQueued(bytes32 proposalId, uint256 eta);

33: event ProposalExecuted(bytes32 proposalId);

36: event ProposalCanceled(bytes32 proposalId);

39: event ProposalVetoed(bytes32 proposalId);

42: event VoteCast(address voter, bytes32 proposalId, uint256 support, uint256 weight, string reason);

45: event VotingDelayUpdated(uint256 prevVotingDelay, uint256 newVotingDelay);

48: event VotingPeriodUpdated(uint256 prevVotingPeriod, uint256 newVotingPeriod);

51: event ProposalThresholdBpsUpdated(uint256 prevBps, uint256 newBps);

54: event QuorumVotesBpsUpdated(uint256 prevBps, uint256 newBps);

57: event VetoerUpdated(address prevVetoer, address newVetoer);
```
​
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/governance/governor/IGovernor.sol

```solidity
File: governance/treasury/ITreasury.sol
​
16: event TransactionScheduled(bytes32 proposalId, uint256 timestamp);

19: event TransactionCanceled(bytes32 proposalId);

22: event TransactionExecuted(bytes32 proposalId, address[] targets, uint256[] values, bytes[] payloads);

25: event DelayUpdated(uint256 prevDelay, uint256 newDelay);

28: event GracePeriodUpdated(uint256 prevGracePeriod, uint256 newGracePeriod);
```
​
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/governance/treasury/ITreasury.sol

```solidity
File: manager/IManager.sol
​
21: event DAODeployed(address token, address metadata, address auction, address treasury, address governor);

26: event UpgradeRegistered(address baseImpl, address upgradeImpl);

31: event UpgradeRemoved(address baseImpl, address upgradeImpl);
```
​
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/manager/IManager.sol

```solidity
File: token/metadata/interfaces/IPropertyIPFSMetadataRenderer.sol
​
16: event PropertyAdded(uint256 id, string name);

19: event ItemAdded(uint256 propertyId, uint256 index);

22: event ContractImageUpdated(string prevImage, string newImage);

25: event RendererBaseUpdated(string prevRendererBase, string newRendererBase);

28: event DescriptionUpdated(string prevDescription, string newDescription);
```
​
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/token/metadata/interfaces/IPropertyIPFSMetadataRenderer.sol

```solidity
File: token/IToken.sol
​
21: event MintScheduled(uint256 baseTokenId, uint256 founderId, Founder founder);
```
​
https://github.com/code-423n4/2022-09-nouns-builder/blob/main/src/token/IToken.sol

### N-02 Use a more recent version of solidity

_There are **20** instances of this issue:_

```solidity
File: lib/interfaces/IEIP712.sol

File: lib/interfaces/IERC1967Upgrade.sol

File: lib/interfaces/IERC721.sol

File: lib/interfaces/IERC721Votes.sol

File: lib/interfaces/IInitializable.sol

File: lib/interfaces/IOwnable.sol

File: lib/interfaces/IPausable.sol

File: lib/proxy/ERC1967Proxy.sol

File: lib/proxy/ERC1967Upgrade.sol

File: lib/proxy/UUPS.sol

File: lib/token/ERC721.sol

File: lib/token/ERC721Votes.sol

File: lib/utils/Address.sol

File: lib/utils/EIP712.sol

File: lib/utils/Initializable.sol

File: lib/utils/Ownable.sol

File: lib/utils/Pausable.sol

File: lib/utils/ReentrancyGuard.sol

File: lib/utils/SafeCast.sol

File: lib/utils/TokenReceiver.sol
```