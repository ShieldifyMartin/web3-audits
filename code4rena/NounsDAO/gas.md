# Nouns DAO

## Gas Optimizations Report

### `++i` costs less gas than `i++` (same for `--i`/`i--`)

Saves 6 gas per instance/loop

_There are **7** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

226: proposalCount++;

292: for (uint256 i = 0; i < proposal.targets.length; i++) {

330: for (uint256 i = 0; i < proposal.targets.length; i++) {

357: for (uint256 i = 0; i < proposal.targets.length; i++) {

382: for (uint256 i = 0; i < proposal.targets.length; i++) {
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

```solidity
File: contracts/NounsToken.sol

151: _mintTo(noundersDAO, _currentNounId++);

153: return _mintTo(minter, _currentNounId++);
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/NounsToken.sol

### Using `!= 0` on `uints` costs less gas than `> 0`

_There are **2** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

541: if (votes > 0) {

967: if (pos > 0 && quorumParamsCheckpoints[pos - 1].fromBlock == blockNumber) {
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied

_There are **5** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

292: for (uint256 i = 0; i < proposal.targets.length; i++) {

330: for (uint256 i = 0; i < proposal.targets.length; i++) {

357: for (uint256 i = 0; i < proposal.targets.length; i++) {

382: for (uint256 i = 0; i < proposal.targets.length; i++) {

948: uint256 lower = 0;
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### Using `private` rather than `public` for constants, saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

_There are **16** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

59: string public constant name = 'Nouns DAO';

62: uint256 public constant MIN_PROPOSAL_THRESHOLD_BPS = 1;

65: uint256 public constant MAX_PROPOSAL_THRESHOLD_BPS = 1_000;

68: uint256 public constant MIN_VOTING_PERIOD = 5_760;

71: uint256 public constant MAX_VOTING_PERIOD = 80_640;

74: uint256 public constant MIN_VOTING_DELAY = 1;

77: uint256 public constant MAX_VOTING_DELAY = 40_320;

80: uint256 public constant MIN_QUORUM_VOTES_BPS_LOWER_BOUND = 200;

83: uint256 public constant MIN_QUORUM_VOTES_BPS_UPPER_BOUND = 2_000;

86: uint256 public constant MAX_QUORUM_VOTES_BPS_UPPER_BOUND = 6_000;

89: uint256 public constant MAX_QUORUM_VOTES_BPS = 2_000;

92: uint256 public constant proposalMaxOperations = 10;

95: uint256 public constant MAX_REFUND_PRIORITY_FEE = 2 gwei;

98: uint256 public constant REFUND_BASE_GAS = 36000;

101: bytes32 public constant DOMAIN_TYPEHASH = keccak256('EIP712Domain(string name,uint256 chainId,address verifyingContract)');

105: bytes32 public constant BALLOT_TYPEHASH = keccak256('Ballot(uint256 proposalId,uint8 support)');
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### Splitting `require()` statements that use `&&` saves gas

Instead of using && on single require check using two require checks can save gas

_There are **9** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

137: require(votingPeriod_ >= MIN_VOTING_PERIOD && votingPeriod_ <= MAX_VOTING_PERIOD, 'NounsDAO::initialize: invalid voting period');

141: require(votingDelay_ >= MIN_VOTING_DELAY && votingDelay_ <= MAX_VOTING_DELAY, 'NounsDAO::initialize: invalid voting delay');

145: require(proposalThresholdBPS_ >= MIN_PROPOSAL_THRESHOLD_BPS && proposalThresholdBPS_ <= MAX_PROPOSAL_THRESHOLD_BPS, 'NounsDAO::initialize: invalid proposal threshold bps');

201: require(targets.length == values.length && targets.length == signatures.length && targets.length == calldatas.length, 'NounsDAO::propose: proposal function information arity mismatch');

623: require(newVotingDelay >= MIN_VOTING_DELAY && newVotingDelay <= MAX_VOTING_DELAY, 'NounsDAO::_setVotingDelay: invalid voting delay');

639: require(newVotingPeriod >= MIN_VOTING_PERIOD && newVotingPeriod <= MAX_VOTING_PERIOD, 'NounsDAO::_setVotingPeriod: invalid voting period');

656: require(newProposalThresholdBPS >= MIN_PROPOSAL_THRESHOLD_BPS && newProposalThresholdBPS <= MAX_PROPOSAL_THRESHOLD_BPS, 'NounsDAO::_setProposalThreshold: invalid proposal threshold bps');

677: require(newMinQuorumVotesBPS >= MIN_QUORUM_VOTES_BPS_LOWER_BOUND && newMinQuorumVotesBPS <= MIN_QUORUM_VOTES_BPS_UPPER_BOUND, 'NounsDAO::_setMinQuorumVotesBPS: invalid min quorum votes bps');

819: require(msg.sender == pendingAdmin && msg.sender != address(0), 'NounsDAO::_acceptAdmin: pending admin only');
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### Use `calldata` instead of `memory` for function parameters

If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **15** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

185: address[] memory targets,

186: uint256[] memory values,

187: string[] memory signatures,

188: bytes[] memory calldatas,

189: string memory description

308: string memory signature,

309: bytes memory data,

407: address[] memory targets,

408: uint256[] memory values,

409: string[] memory signatures,

410: bytes[] memory calldatas

536: string memory reason

906: DynamicQuorumParams memory params

964: function _writeQuorumParamsCheckpoint(DynamicQuorumParams memory params) internal {

1018: function safe32(uint256 n, string memory errorMessage) internal pure returns (uint32) {
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **9** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

138: votingPeriod_ >= MIN_VOTING_PERIOD && votingPeriod_ <= MAX_VOTING_PERIOD,

142: votingDelay_ >= MIN_VOTING_DELAY && votingDelay_ <= MAX_VOTING_DELAY,

146: proposalThresholdBPS_ >= MIN_PROPOSAL_THRESHOLD_BPS && proposalThresholdBPS_ <= MAX_PROPOSAL_THRESHOLD_BPS,

433: require(proposalCount >= proposalId, 'NounsDAO::state: invalid proposal id');

449: } else if (block.timestamp >= proposal.eta + timelock.GRACE_PERIOD()) {

624: newVotingDelay >= MIN_VOTING_DELAY && newVotingDelay <= MAX_VOTING_DELAY,

640: newVotingPeriod >= MIN_VOTING_PERIOD && newVotingPeriod <= MAX_VOTING_PERIOD,

657: newProposalThresholdBPS >= MIN_PROPOSAL_THRESHOLD_BPS

678: newMinQuorumVotesBPS >= MIN_QUORUM_VOTES_BPS_LOWER_BOUND
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### Not using the named return variables when a function returns wastes deployment gas

_There are **4** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

407: address[] memory targets,

408: uint256[] memory values,

409: string[] memory signatures,

410: bytes[] memory calldatas

414: return (p.targets, p.values, p.signatures, p.calldatas);
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### `public` functions not called by the contract should be declared `external` instead

_There are **5** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

124: function initialize( address timelock_, address nouns_, address vetoer_, uint256 votingPeriod_, uint256 votingDelay_, uint256 proposalThresholdBPS_, DynamicQuorumParams calldata dynamicQuorumParams_ ) public virtual {

184: function propose( address[] memory targets, uint256[] memory values, string[] memory signatures, bytes[] memory calldatas, string memory description ) public returns (uint256) {

851: function _burnVetoPower() public {

862: function proposalThreshold() public  view  returns (uint256) {

1002: function maxQuorumVotes() public  view  returns (uint256) {
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### Don't compare boolean expressions to boolean literals

Use if(x) / if(!x) instead of if(x == true) / if(x == false)

_There are **1** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

597: require(receipt.hasVoted == false, 'NounsDAO::castVoteInternal: voter already voted');
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### Expressions for constant values such as a call to `keccak256()`, should use `immutable` rather than `constant`

It is expected that the value should be converted into a constant value at compile time. But actually the expression is re-calculated each time the constant is referenced./

_There are **1** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

101: bytes32 public constant DOMAIN_TYPEHASH = keccak256('EIP712Domain(string name,uint256 chainId,address verifyingContract)');
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### Use custom errors rather than `revert()`/`require()` strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **44** instances of this issue:_

```solidity
File: contracts/governance/NounsDAOLogicV2.sol

133: require(address(timelock) == address(0), 'NounsDAO::initialize: can only initialize once');

134: require(msg.sender == admin, 'NounsDAO::initialize: admin only');

135: require(timelock_ != address(0), 'NounsDAO::initialize: invalid timelock address');

136: require(nouns_ != address(0), 'NounsDAO::initialize: invalid nouns address');

137: require( votingPeriod_ >= MIN_VOTING_PERIOD && votingPeriod_ <= MAX_VOTING_PERIOD, 'NounsDAO::initialize: invalid voting period' );

141: require(votingDelay_ >= MIN_VOTING_DELAY && votingDelay_ <= MAX_VOTING_DELAY, 'NounsDAO::initialize: invalid voting delay');

145: require(proposalThresholdBPS_ >= MIN_PROPOSAL_THRESHOLD_BPS && proposalThresholdBPS_ <= MAX_PROPOSAL_THRESHOLD_BPS, 'NounsDAO::initialize: invalid proposal threshold bps');

197: require(nouns.getPriorVotes(msg.sender, block.number - 1) > temp.proposalThreshold, 'NounsDAO::propose: proposer votes below proposal threshold');

201: require(targets.length == values.length && targets.length == signatures.length && targets.length == calldatas.length, 'NounsDAO::propose: proposal function information arity mismatch');

207: require(targets.length != 0, 'NounsDAO::propose: must provide actions');

208: require(targets.length <= proposalMaxOperations, 'NounsDAO::propose: too many actions');

213: require(proposersLatestProposalState != ProposalState.Active, 'NounsDAO::propose: one live proposal per proposer, found an already active proposal');

217: require(proposersLatestProposalState != ProposalState.Pending, 'NounsDAO::propose: one live proposal per proposer, found an already pending proposal');

286: require(state(proposalId) == ProposalState.Succeeded, 'NounsDAO::queue: proposal can only be queued if it is succeeded');

312: require(!timelock.queuedTransactions(keccak256(abi.encode(target, value, signature, data, eta))), 'NounsDAO::queueOrRevertInternal: identical proposal action already queued at eta');

324: require(state(proposalId) == ProposalState.Queued, 'NounsDAO::execute: proposal can only be executed if it is queued');

347: require(state(proposalId) != ProposalState.Executed, 'NounsDAO::cancel: cannot cancel executed proposal');

350: require( msg.sender == proposal.proposer || nouns.getPriorVotes(proposal.proposer, block.number - 1) < proposal.proposalThreshold, 'NounsDAO::cancel: proposer above threshold' );

375: require(vetoer != address(0), 'NounsDAO::veto: veto power burned');

376: require(msg.sender == vetoer, 'NounsDAO::veto: only vetoer');

377: require(state(proposalId) != ProposalState.Executed, 'NounsDAO::veto: cannot veto executed proposal');

433: require(proposalCount >= proposalId, 'NounsDAO::state: invalid proposal id');

577: require(signatory != address(0), 'NounsDAO::castVoteBySig: invalid signature');

593: require(state(proposalId) == ProposalState.Active, 'NounsDAO::castVoteInternal: voting is closed');

594: require(support <=  2, 'NounsDAO::castVoteInternal: invalid vote type');

597: require(receipt.hasVoted == false, 'NounsDAO::castVoteInternal: voter already voted');

622: require(msg.sender == admin, 'NounsDAO::_setVotingDelay: admin only');

623: require(newVotingDelay >= MIN_VOTING_DELAY && newVotingDelay <= MAX_VOTING_DELAY, 'NounsDAO::_setVotingDelay: invalid voting delay');

638: require(msg.sender == admin, 'NounsDAO::_setVotingPeriod: admin only');

639: require(newVotingPeriod >= MIN_VOTING_PERIOD && newVotingPeriod <= MAX_VOTING_PERIOD, 'NounsDAO::_setVotingPeriod: invalid voting period');

655: require(msg.sender == admin, 'NounsDAO::_setProposalThresholdBPS: admin only');

656: require(newProposalThresholdBPS >= MIN_PROPOSAL_THRESHOLD_BPS && newProposalThresholdBPS <= MAX_PROPOSAL_THRESHOLD_BPS, 'NounsDAO::_setProposalThreshold: invalid proposal threshold bps');

674: require(msg.sender  == admin, 'NounsDAO::_setMinQuorumVotesBPS: admin only');

677: require( newMinQuorumVotesBPS >= MIN_QUORUM_VOTES_BPS_LOWER_BOUND && newMinQuorumVotesBPS <= MIN_QUORUM_VOTES_BPS_UPPER_BOUND, 'NounsDAO::_setMinQuorumVotesBPS: invalid min quorum votes bps' );

682: require(newMinQuorumVotesBPS <= params.maxQuorumVotesBPS, 'NounsDAO::_setMinQuorumVotesBPS: min quorum votes bps greater than max');

702: require(msg.sender  == admin, 'NounsDAO::_setMaxQuorumVotesBPS: admin only');

705: require(newMaxQuorumVotesBPS <= MAX_QUORUM_VOTES_BPS_UPPER_BOUND, 'NounsDAO::_setMaxQuorumVotesBPS: invalid max quorum votes bps');

709: require(params.minQuorumVotesBPS <= newMaxQuorumVotesBPS, 'NounsDAO::_setMaxQuorumVotesBPS: min quorum votes bps greater than max');

727: require(msg.sender == admin, 'NounsDAO::_setQuorumCoefficient: admin only');

801: require(msg.sender  == admin, 'NounsDAO::_setPendingAdmin: admin only');

819: require(msg.sender == pendingAdmin && msg.sender != address(0), 'NounsDAO::_acceptAdmin: pending admin only');

840: require(msg.sender == vetoer, 'NounsDAO::_setVetoer: vetoer only');

853: require(msg.sender == vetoer, 'NounsDAO::_burnVetoPower: vetoer only');

1019: require(n <= type(uint32).max, errorMessage);
```

https://github.com/code-423n4/2022-08-nounsdao/blob/main/contracts/governance/NounsDAOLogicV2.sol

### Use a more recent version of solidity

- Use a solidity version of at least 0.8.4 to get custom errors, which are cheaper at deployment than revert()/require() strings \
- Use a solidity version of at least 0.8.10 to have external calls skip contract existence checks if the external call has a return value
