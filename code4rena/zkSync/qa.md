# zkSync

## QA Report

### L-01 `require()` should be used instead of `assert()`

_There are **1** instances of this issue:_

```solidity
File: /ethereum/contracts/zksync/facets/DiamondCut.sol

16: assert(SECURITY_COUNCIL_APPROVALS_FOR_EMERGENCY_UPGRADE > 0);
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/DiamondCut.sol

### L-02 `emit` function called early

_There are **1** instances of this issue:_

```solidity
File: /ethereum/contracts/zksync/facets/Executor.sol

269: emit BlocksVerification(s.totalBlocksVerified, currentTotalBlocksVerified);
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/Executor.sol

https://github.com/code-423n4/2022-10-zksync/blob/main/zksync/contracts/bridge/L2StandardERC20.sol

### L-03 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **1** instances of this issue:_

```solidity
File: /zksync/contracts/bridge/L2StandardERC20.sol

103: _mint(_to, _amount);
```

https://github.com/code-423n4/2022-10-zksync/blob/main/zksync/contracts/bridge/L2StandardERC20.sol

### L-04 Missing checks for `address`(0x0) when assigning values to `address` state variables

_There are **3** instances of this issue:_

```solidity
File: /ethereum/contracts/zksync/facets/Getters.sol

172: function facetFunctionSelectors(address _facet) external view returns (bytes4[] memory) {
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/Getters.sol

```solidity
File: /ethereum/contracts/zksync/facets/Governance.sol

15: function setPendingGovernor(address _newPendingGovernor) external onlyGovernor {

45: function setValidator(address _validator, bool _active) external onlyGovernor {
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/Governance.sol

### N-01 Event is missing `indexed` fields

Each `event` should use three `indexed` fields if there are three or more fields

_There are **10** instances of this issue:_

```solidity
File: /ethereum/contracts/zksync/interfaces/IDiamondCut.sol

20: event DiamondCutProposal(Diamond.FacetCut[] _facetCuts, address _initAddress);

22: event DiamondCutProposalCancelation(uint256 currentProposalId, bytes32 indexed proposedDiamondCutHash);

24: event DiamondCutProposalExecution(Diamond.DiamondCutData _diamondCut);

26: event EmergencyFreeze();

28: event Unfreeze(uint256 lastDiamondFreezeTimestamp);
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/interfaces/IDiamondCut.sol

```solidity
File: /ethereum/contracts/zksync/interfaces/IExecutor.sol

85: event BlocksRevert(uint256 totalBlocksCommitted, uint256 totalBlocksVerified, uint256 totalBlocksExecuted);
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/interfaces/IExecutor.sol

```solidity
File: /ethereum/contracts/zksync/interfaces/IGovernance.sol

32: event IsPorterAvailableStatusUpdate(bool isPorterAvailable);

48: event NewVerifierParams(VerifierParams oldVerifierParams, VerifierParams newVerifierParams);
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/interfaces/IGovernance.sol

```solidity
File: /ethereum/contracts/zksync/interfaces/IMailbox.sol

95: event NewPriorityRequest(
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/interfaces/IMailbox.sol

```solidity
File: /ethereum/contracts/zksync/libraries/Diamond.sol

16: event DiamondCut(FacetCut[] facetCuts, address initAddress, bytes initCalldata);
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/libraries/Diamond.sol

### N-02 `public` functions not called by the contract should be declared `external` instead

_There are **4** instances of this issue:_

```solidity
File: /ethereum/contracts/bridge/L1ERC20Bridge.sol

282: function l2TokenAddress(address _l1Token) public view returns (address) {
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/bridge/L1ERC20Bridge.sol

```solidity
File: /ethereum/contracts/bridge/L1EthBridge.sol

243: function l2TokenAddress(address) public pure returns (address) {
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/bridge/L1EthBridge.sol

```solidity
File: /zksync/contracts/bridge/L2ETHBridge.sol

79: function l2TokenAddress(address) public pure returns (address) {

84: function l1TokenAddress(address) public pure override returns (address) {
```

https://github.com/code-423n4/2022-10-zksync/blob/main/zksync/contracts/bridge/L2ETHBridge.sol

### N-03 Non-floating `pragma` should be used

_There are **30** instances of this issue:_

### N-04 Use a more recent version of solidity

_There are **30** instances of this issue:_

### N-05 Open TODOs

_There are **5** instances of this issue:_

```solidity
File: /ethereum/contracts/zksync/facets/Mailbox.sol

94: // TODO: estimate gas for L1 execute

127: // TODO: Restore after stable priority op fee modeling. (SMA-1230)

169: layer2Tip: uint192(0) // TODO: Restore after fee modeling will be stable. (SMA-1230)
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/Mailbox.sol

```solidity
File: /ethereum/contracts/zksync/interfaces/IExecutor.sol

56: /// TODO: The verifier integration is not finished yet, change the structure for compatibility later
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/interfaces/IExecutor.sol

```solidity
File: /ethereum/contracts/zksync/Config.sol

28: // TODO: change constant to the real root hash of empty Merkle tree (SMA-184)
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/Config.sol

### N-06 `require()`/`revert()` statements should have descriptive reason strings

_There are **106** instances of this issue:_

```solidity
File: /ethereum/contracts/zksync/libraries/Diamond.sol

100: require(selectors.length > 0, "B"); // no functions for diamond cut

126: require(_facet != address(0), "G"); // facet with zero address cannot be added

135: require(oldFacet.facetAddress == address(0), "J"); // facet for this selector already exists

150: require(_facet != address(0), "K"); // cannot replace facet with zero address

156: require(oldFacet.facetAddress != address(0), "L"); // it is impossible to replace the facet with zero address

170: require(_facet == address(0), "a1"); // facet address must be zero

176: require(oldFacet.facetAddress != address(0), "a2"); // Can't delete a non-existent facet

214: require(_isSelectorFreezable == ds.selectorToFacet[selector0].isFreezable, "J1");

279: require(_calldata.length == 0, "H"); // Non-empty calldata for zero address

283: require(success, "I"); // delegatecall failed

287: require(data.length == 32, "lp");

288: require(abi.decode(data, (bytes32)) == DIAMOND_INIT_SUCCESS_RETURN_VALUE, "lp1");
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/libraries/Diamond.sol

```solidity
File: /ethereum/contracts/zksync/libraries/Merkle.sol

23: require(pathLength > 0, "xc");

24: require(pathLength < 256, "bt");

25: require(_index < 2**pathLength, "pz");
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/libraries/Merkle.sol

```solidity
File: /ethereum/contracts/zksync/libraries/PriorityQueue.sol

64: require(!_queue.isEmpty(), "D"); // priority queue is empty

72: require(!_queue.isEmpty(), "s"); // priority queue is empty
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/libraries/PriorityQueue.sol

```solidity
File: /ethereum/contracts/zksync/facets/Base.sol

16: require(msg.sender == s.governor, "1g"); // only by governor

22: require(s.validators[msg.sender], "1h"); // validator is not active
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/Base.sol

```solidity
File: /ethereum/contracts/zksync/facets/DiamondCut.sol

23: require(s.diamondCutStorage.proposedDiamondCutTimestamp == 0, "a3"); // proposal already exists

40: require(_resetProposal(), "g1"); // failed cancel diamond cut

55: require(approvedBySecurityCouncil || upgradeNoticePeriodPassed, "a6"); // notice period should expire

56: require(approvedBySecurityCouncil || !diamondStorage.isFrozen, "f3");

59: require(

65: require(_resetProposal(), "a5"); // failed reset proposal

80: require(!diamondStorage.isFrozen, "a9"); // diamond proxy is frozen already

94: require(diamondStorage.isFrozen, "a7"); // diamond proxy is not frozen

106: require(s.diamondCutStorage.securityCouncilMembers[msg.sender], "a9"); // not a security council member

108: require(s.diamondCutStorage.securityCouncilMemberLastApprovedProposalId[msg.sender] < currentProposalId, "ao"); // already approved this proposal

111: require(s.diamondCutStorage.proposedDiamondCutTimestamp != 0, "f0"); // there is no proposed diamond cut

112: require(s.diamondCutStorage.proposedDiamondCutHash == _diamondCutHash, "f1"); // proposed diamond cut do not match to the approved
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/DiamondCut.sol

```solidity
File: /ethereum/contracts/zksync/facets/Executor.sol

28: require(_newBlock.blockNumber == _previousBlock.blockNumber + 1, "f"); // only commit next block

39: require(_previousBlock.blockHash == previousBlockHash, "l");

41: require(expectedPriorityOperationsHash == _newBlock.priorityOperationsHash, "t");

43: require(expectedNumberOfLayer1Txs == _newBlock.numberOfLayer1Txs);

45: require(l2BlockTimestamp == _newBlock.timestamp);

52: require(timestampNotTooSmall, "h"); // New block timestamp is too small

53: require(timestampNotTooBig, "h1"); // New block timestamp is too big

57: require(

117: require(keccak256(l2Messages[currentMessage]) == hashedMessage, "k2");

129: require(!isSystemContextLogProcessed, "fx");

136: require(bytecodeHash == L2ContractHelper.hashL2Bytecode(factoryDeps[currentBytecode]), "k3");

142: require(currentBytecode == factoryDeps.length, "ym");

143: require(currentMessage == l2Messages.length, "pl");

145: require(isSystemContextLogProcessed, "by");

159: require(s.storedBlockHashes[s.totalBlocksCommitted] == _hashStoredBlockInfo(_lastCommittedBlockData), "i"); // incorrect previous block data

192: require(currentBlockNumber == s.totalBlocksExecuted + _executedBlockIdx + 1, "k"); // Execute blocks in order

193: require(

199: require(priorityOperationsHash == _storedBlock.priorityOperationsHash, "x"); // priority operations hash does not match to expected

216: require(s.totalBlocksExecuted <= s.totalBlocksVerified, "n");

237: require(_hashStoredBlockInfo(_prevBlock) == s.storedBlockHashes[currentTotalBlocksVerified], "t1");

241: require(

261: require(successVerifyProof, "p"); // Proof verification fail

265: require(successProofAggregation, "hh"); // Proof aggregation must be valid

268: require(currentTotalBlocksVerified <= s.totalBlocksCommitted, "q");

297: require(_recurisiveAggregationInput.length == 4);

337: require(s.totalBlocksCommitted > _newLastBlock, "v1"); // the last committed block is less new last block
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/Executor.sol

```solidity
File: /ethereum/contracts/zksync/facets/Getters.sol

148: require(ds.selectorToFacet[_selector].facetAddress != address(0), "g2");
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/Getters.sol

```solidity
File: /ethereum/contracts/zksync/facets/Governance.sol

30: require(msg.sender == pendingGovernor, "n4"); // Only proposed by current governor address can claim the governor rights
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/Governance.sol

```solidity
File: /ethereum/contracts/zksync/facets/Mailbox.sol

56: require(_blockNumber <= s.totalBlocksExecuted, "xx");

63: require(hashedLog != L2_L1_LOGS_TREE_DEFAULT_LEAF_HASH, "tw");

66: require(_proof.length == L2_TO_L1_LOG_MERKLE_TREE_HEIGHT, "rz");

124: require(_ergsLimit <= PRIORITY_TX_MAX_ERGS_LIMIT, "ui");
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/facets/Mailbox.sol

```solidity
File: /ethereum/contracts/zksync/DiamondProxy.sol

13: require(_chainId == block.chainid, "pr");

24: require(msg.data.length >= 4 || msg.data.length == 0, "Ut");

29: require(facetAddress != address(0), "F"); // Proxy has no facet for this selector

30: require(!diamondStorage.isFrozen || !facet.isFreezable, "q1"); // Facet is frozen
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/zksync/DiamondProxy.sol

```solidity
File: /ethereum/contracts/bridge/L1ERC20Bridge.sol

77: require(_factoryDeps.length == 2, "mk");

117: require(amount > 0, "1T"); // empty deposit amount

207: require(success, "yn");

210: require(amount > 0, "y1");

232: require(!isWithdrawalFinalized[_l2BlockNumber][_l2MessageIndex], "pw");

249: require(success, "nq");

271: require(_l2ToL1message.length == 76, "kk");

274: require(bytes4(functionSignature) == this.finalizeWithdrawal.selector, "nt");
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/bridge/L1ERC20Bridge.sol

```solidity
File: /ethereum/contracts/bridge/L1EthBridge.sol

93: require(_l1Token == CONVENTIONAL_ETH_ADDRESS, "bx");

141: require(_l1Token == CONVENTIONAL_ETH_ADDRESS, "sj");

145: require(amount != 0);

166: require(success, "ju");

189: require(!isWithdrawalFinalized[_l2BlockNumber][_l2MessageIndex], "jj");

205: require(success, "rj");

221: require(_message.length == 56);

224: require(bytes4(functionSignature) == this.finalizeWithdrawal.selector);

238: require(callSuccess);
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/bridge/L1EthBridge.sol

```solidity
File: /ethereum/contracts/common/AllowList.sol

33: require(_owner != address(0), "kq");

38: require(msg.sender == owner, "kx");

67: require(targetsLength == _enables.length, "yg"); // The size of arrays should be equal

96: require(

155: require(msg.sender == newOwner, "n0"); // Only proposed by current owner address can claim the owner rights
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/common/AllowList.sol

```solidity
File: /ethereum/contracts/common/AllowListed.sol

15: require(_allowList.canCall(msg.sender, address(this), functionSig), "nr");
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/common/AllowListed.sol

```solidity
File: /ethereum/contracts/common/L2ContractHelper.sol

50: require(_bytecode.length % 32 == 0, "po");

53: require(bytecodeLenInWords < 2**16, "pp"); // bytecode length must be less than 2^16 words

54: require(bytecodeLenInWords % 2 == 1, "pr"); // bytecode length in words must be odd

65: require(version == 1 && _bytecodeHash[1] == bytes1(0), "zf"); // Incorrectly formatted bytecodeHash

67: require(bytecodeLen(_bytecodeHash) % 2 == 1, "uy"); // Code length in words must be odd
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/common/L2ContractHelper.sol

```solidity
File: /ethereum/contracts/common/ReentrancyGuard.sol

55: require(lockSlotOldValue == 0, "1B");

72: require(_status == _NOT_ENTERED, "r1");
```

https://github.com/code-423n4/2022-10-zksync/blob/main/ethereum/contracts/common/ReentrancyGuard.sol

```solidity
File: /zksync/contracts/bridge/L2ERC20Bridge.sol

58: require(msg.sender == l1Bridge, "mq");

63: require(deployedToken == expectedL2Token, "mt");

95: require(l1Token != address(0), "yh");
```

https://github.com/code-423n4/2022-10-zksync/blob/main/zksync/contracts/bridge/L2ERC20Bridge.sol

```solidity
File: /zksync/contracts/bridge/L2ETHBridge.sol

49: require(msg.sender == l1Bridge, "ni");

50: require(_l1Token == CONVENTIONAL_ETH_ADDRESS);

64: require(_l2Token == CONVENTIONAL_ETH_ADDRESS, "zn");
```

https://github.com/code-423n4/2022-10-zksync/blob/main/zksync/contracts/bridge/L2ETHBridge.sol

```solidity
File: /zksync/contracts/bridge/L2StandardERC20.sol

44: require(l1Address == address(0), "in5"); // Is already initialized

45: require(_l1Address != address(0), "in6"); // Should be non-zero address

96: require(msg.sender == l2Bridge);
```

https://github.com/code-423n4/2022-10-zksync/blob/main/zksync/contracts/bridge/L2StandardERC20.sol
