#  Olympus DAO
## Gas Optimizations Report

###  `++i`/`--i` costs less gas than `i++`/`i--`
Saves 6 gas per instance/loop. There is inconsistency with the other contracts.

_There are **7** instances of this issue:_

```solidity
File: utils/KernelUtils.sol

49: i++;

64: i++;
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/utils/KernelUtils.sol

```solidity
File: policies/Operator.sol

488: decimals++;

670: _status.low.count++;

675: _status.low.count--;

686: _status.high.count++;

691: _status.low.count--;
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/Operator.sol

###  It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied

_There are **3** instances of this issue:_

```solidity
File: utils/KernelUtils.sol

43: for (uint256 i = 0; i < 5; ) {

58: for (uint256 i = 0; i < 32; ) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/utils/KernelUtils.sol

```solidity
File: Kernel.sol

397: for (uint256 i = 0; i < reqLength; ) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/Kernel.sol

###  Using `private` rather than `public` for constants, saves gas
If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

_There are **1** instances of this issue:_

```solidity
File: modules/RANGE.sol

65: uint256 public constant FACTOR_SCALE = 1e4;
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/RANGE.sol

###  Use `calldata` instead of `memory` for function parameters
If a reference type function parameter is read-only, it is cheaper in gas to use calldata instead of memory. Calldata is a non-modifiable, non-persistent area where function arguments are stored, and behaves mostly like memory. Try to use calldata as a data location because it will avoid copies and also makes sure that the data cannot be modified.

_There are **3** instances of this issue:_

```solidity
File: policies/TreasuryCustodian.sol

53: function revokePolicyApprovals(address policy_, ERC20[] memory tokens_) external {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/TreasuryCustodian.sol

```solidity
File: modules/RANGE.sol

79: ERC20[2] memory tokens_,

80: uint256[3] memory rangeParams_
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/RANGE.sol

###  Not using the named return variables when a function returns wastes deployment gas


_There are **6** instances of this issue:_

```solidity
File: modules/MINTR.sol

25: function VERSION() external pure override returns (uint8 major, uint8 minor) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/MINTR.sol

```solidity
File: modules/RANGE.sol

115: function VERSION() external pure override returns (uint8 major, uint8 minor) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/RANGE.sol

```solidity
File: modules/VOTES.sol

27: function VERSION() external pure override returns (uint8 major, uint8 minor) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/VOTES.sol

```solidity
File: modules/INSTR.sol

28: function VERSION() public pure override returns (uint8 major, uint8 minor) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/INSTR.sol

```solidity
File: modules/TRSRY.sol

51: function VERSION() external pure override returns (uint8 major, uint8 minor) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/TRSRY.sol

```solidity
File: policies/BondCallback.sol

173: function amountsForMarket(uint256 id_) external view override returns (uint256 in_, uint256 out_) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/BondCallback.sol

###  `public` functions not called by the contract should be declared `external` instead


_There are **16** instances of this issue:_

```solidity
File: Kernel.sol

439: function grantRole(Role role_, address addr_) public onlyAdmin {

451: function revokeRole(Role role_, address addr_) public onlyAdmin {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/Kernel.sol

```solidity
File: modules/TRSRY.sol

75: function withdrawReserves(address to_, ERC20 token_, uint256 amount_) public {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/TRSRY.sol

```solidity
File: modules/INSTR.sol

23: function KEYCODE() public pure override returns (Keycode) {

28: function VERSION() public pure override returns (uint8 major, uint8 minor) {

37: function getInstructions(uint256 instructionsId_) public view returns (Instruction[] memory) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/INSTR.sol

```solidity
File: modules/VOTES.sol

22: function KEYCODE() public pure override returns (Keycode) {

45: function transfer(address to_, uint256 amount_) public pure override returns (bool) {

51: function transferFrom(address from_, address to_, uint256 amount_) public override permissioned returns (bool) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/VOTES.sol

```solidity
File: modules/RANGE.sol

110: function KEYCODE() public pure override returns (Keycode) {

215: function updateMarket(bool high_, uint256 market_, uint256 marketCapacity_) public permissioned {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/RANGE.sol

```solidity
File: modules/MINTR.sol

20: function KEYCODE() public pure override returns (Keycode) {

33: function mintOhm(address to_, uint256 amount_) public permissioned {

37: function burnOhm(address from_, uint256 amount_) public permissioned {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/MINTR.sol

```solidity
File: policies/Governance.sol

145: function getMetadata(uint256 proposalId_) public view returns (ProposalMetadata memory) {

151: function getActiveProposal() public view returns (ActivatedProposal memory) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/Governance.sol


### `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables


_There are **16** instances of this issue:_

```solidity
File: modules/VOTES.sol

56: balanceOf[from_] -= amount_;

58: balanceOf[to_] += amount_;
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/VOTES.sol

```solidity
File: policies/BondCallback.sol

143: _amountsPerMarket[id_][0] += inputAmount_;

144: _amountsPerMarket[id_][1] += outputAmount_;
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/BondCallback.sol

```solidity
File: modules/TRSRY.sol

96: reserveDebt[token_][msg.sender] += amount_;

97: totalDebt[token_] += amount_;

115: reserveDebt[token_][msg.sender] -= received;

116: totalDebt[token_] -= received;

132: else totalDebt[token_] -= oldDebt - amount_;
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/TRSRY.sol

```solidity
File: policies/Governance.sol

194: totalEndorsementsForProposal[proposalId_] -= previousEndorsement;

198: totalEndorsementsForProposal[proposalId_] += userVotes;

252: yesVotesForProposal[activeProposal.proposalId] += userVotes;

254: noVotesForProposal[activeProposal.proposalId] += userVotes;
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/Governance.sol

```solidity
File: modules/PRICE.sol

136: _movingAverage += (currentPrice - earliestPrice) / numObs;

138: _movingAverage -= (earliestPrice - currentPrice) / numObs;

222: total += startObservations_[i];
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/modules/PRICE.sol

### Functions that are access-restricted from most users may be marked as `payable`
Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **4** instances of this issue:_

```solidity
File: Kernel.sol

235: function executeAction(Actions action_, address target_) external onlyExecutor {

439: function grantRole(Role role_, address addr_) public onlyAdmin {

451: function revokeRole(Role role_, address addr_) public onlyAdmin {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/Kernel.sol

```solidity
File: policies/BondCallback.sol

190: function setOperator(Operator operator_) external onlyRole("callback_admin") {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/BondCallback.sol


### Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`
In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **5** instances of this issue:_

```solidity
File: Operator.sol

209: if (uint48(block.timestamp) >= RANGE.lastActive(true) + uint48(config_.regenWait) && _status.high.count >= config_.regenThreshold) {

215: if ( uint48(block.timestamp) >= RANGE.lastActive(false) + uint48(config_.regenWait) && _status.low.count >= config_.regenThreshold ) { _regenerate(false); }

486: while (price_ >= 10) {

667: if (currentPrice >= movingAverage) {

683: if (currentPrice <= movingAverage) {
```

https://github.com/code-423n4/2022-08-olympus/blob/main/src/policies/Operator.sol
