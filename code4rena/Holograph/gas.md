# Holograph

## Gas Optimizations Report

### G-01 It costs more gas to initialize non-`constant`/non-`immutable` variables to zero than to let the default of zero be applied

Not overwriting the default for stack variables saves 8 gas. Storage and memory variables have larger savings

_There are **19** instances of this issue:_

```solidity
File: /contracts/HolographBridge.sol

380: uint256 fee = 0;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol

```solidity
File: /contracts/HolographOperator.sol

310: uint256 gasLimit = 0;

311: uint256 gasPrice = 0;

781: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/enforcer/PA1D.sol

307: for (uint256 i = 0; i < length; i++) {

323: for (uint256 i = 0; i < length; i++) {

340: for (uint256 i = 0; i < length; i++) {

356: for (uint256 i = 0; i < length; i++) {

394: for (uint256 i = 0; i < length; i++) {

414: for (uint256 i = 0; i < length; i++) {

432: for (uint256 t = 0; t < tokenAddresses.length; t++) {

437: for (uint256 i = 0; i < addresses.length; i++) {

454: for (uint256 i = 0; i < addresses.length; i++) {

474: for (uint256 i = 0; i < addresses.length; i++) {

667: bidShares.prevOwner.value = 0;

668: bidShares.owner.value = 0;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```solidity
File: /contracts/enforcer/HolographERC721.sol

357: for (uint256 i = 0; i < length; i++) {

716: for (uint256 i = 0; i < length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```solidity
File: /contracts/enforcer/HolographERC20.sol

564: for (uint256 i = 0; i < wallets.length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

### G-02 Use custom errors rather than `revert()`/`require()` strings to save gas

Custom errors are available from solidity version 0.8.4. Custom errors save ~50 gas each time they're hitby avoiding having to allocate and store the revert string. Not defining the strings also save deployment gas

_There are **151** instances of this issue:_

```solidity
File: /contracts/HolographBridge.sol

148: require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");

163: require(!_isInitialized(), "HOLOGRAPH: already initialized");

203: require(

214: require(selector == Holographable.bridgeIn.selector, "HOLOGRAPH: bridge in failed");

224: require(

233: require(doNotRevert, "HOLOGRAPH: reverted");

255: require(

270: require(selector == Holographable.bridgeOut.selector, "HOLOGRAPH: bridge out failed");

352: require(
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol

```solidity
File: /contracts/HolographOperator.sol

241: require(!_isInitialized(), "HOLOGRAPH: already initialized");

309: require(_operatorJobs[hash] > 0, "HOLOGRAPH: invalid job");

350: require(timeDifference > 0, "HOLOGRAPH: operator has time");

354: require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

368: require(fallbackOperator == msg.sender, "HOLOGRAPH: invalid fallback");

415: require(gasleft() > gasLimit, "HOLOGRAPH: not enough gas left");

446: require(msg.sender == address(this), "HOLOGRAPH: operator only call");

485: require(msg.sender == address(_messagingModule()), "HOLOGRAPH: messaging only call");

591: require(msg.sender == _bridge(), "HOLOGRAPH: bridge only call");

595: require(hlgFee < msg.value, "HOLOGRAPH: not enough value");

728: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

739: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

756: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

829: require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

839: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

857: require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");

863: require(current <= amount, "HOLOGRAPH: bond amount too small");

881: require(_operatorPods[pod - 1].length < type(uint16).max, "HOLOGRAPH: too many operators");

889: require(_utilityToken().transferFrom(msg.sender, address(this), amount), "HOLOGRAPH: token transfer failed");

903: require(_bondedOperators[operator] != 0, "HOLOGRAPH: operator not bonded");

911: require(_isContract(operator), "HOLOGRAPH: operator not contract");

915: require(Ownable(operator).isOwner(msg.sender), "HOLOGRAPH: sender not owner");

932: require(_utilityToken().transfer(recipient, amount), "HOLOGRAPH: token transfer failed");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/HolographFactory.sol

144: require(!_isInitialized(), "HOLOGRAPH: already initialized");

220: require(_verifySigner(signature.r, signature.s, signature.v, hash, signer), "HOLOGRAPH: invalid signature");

228: require(!_isContract(holographerAddress), "HOLOGRAPH: already deployed");

250: require(
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```solidity
File: /contracts/module/LayerZeroModule.sol

159: require(!_isInitialized(), "HOLOGRAPH: already initialized");

235: require(msg.sender == address(_operator()), "HOLOGRAPH: operator only call");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol

```solidity
File: /contracts/enforcer/Holographer.sol

148: require(!_isInitialized(), "HOLOGRAPHER: already initialized");

166: require(success && selector == InitializableInterface.init.selector, "initialization failed");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/Holographer.sol

```solidity
File: /contracts/enforcer/PA1D.sol

159: require(isOwner(), "PA1D: caller not an owner");

174: require(!_isInitialized(), "PA1D: already initialized");

190: require(initialized == 0, "PA1D: already initialized");

390: require(balance - gasCost > 10000, "PA1D: Not enough ETH to transfer");

411: require(balance > 10000, "PA1D: Not enough tokens to transfer");

416: require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

435: require(balance > 10000, "PA1D: Not enough tokens to transfer");

439: require(erc20.transfer(addresses[i], sending), "PA1D: Couldn't transfer token");

460: require(matched, "PA1D: sender not authorized");

472: require(addresses.length == bps.length, "PA1D: missmatched array lenghts");

477: require(totalBp == 10000, "PA1D: bps down't equal 10000");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```solidity
File: /contracts/enforcer/HolographERC721.sol

212: require(msg.sender == _holograph().getBridge(), "ERC721: bridge only call");

224: require(msg.sender == sourceContract, "ERC721: source only call");

239: require(!_isInitialized(), "ERC721: already initialized");

258: require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC721: could not init source");

263: require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

323: require(_exists(tokenId), "ERC721: token does not exist");

370:  require(to != tokenOwner, "ERC721: cannot approve self");

371: require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

373: require(SourceERC721().beforeApprove(tokenOwner, to, tokenId));

378: require(SourceERC721().afterApprove(tokenOwner, to, tokenId));

388: require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

391: require(SourceERC721().beforeBurn(wallet, tokenId));

395: require(SourceERC721().afterBurn(wallet, tokenId));

404: require(!_exists(tokenId), "ERC721: token already exists");

408: require(SourceERC721().bridgeIn(fromChain, from, to, tokenId, data), "HOLOGRAPH: bridge in failed");

419: require(to != address(0), "ERC721: zero address");

420: require(_isApproved(sender, tokenId), "ERC721: sender not approved");

421: require(from == _tokenOwner[tokenId], "ERC721: from is not owner");

458: require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

460: require(SourceERC721().beforeSafeTransfer(from, to, tokenId, data));

464: require(

473: require(SourceERC721().afterSafeTransfer(from, to, tokenId, data));

484: require(to != msg.sender, "ERC721: cannot approve self");

486: require(SourceERC721().beforeApprovalAll(to, approved));

491: require(SourceERC721().afterApprovalAll(to, approved));

513: require(!_burnedTokens[token], "ERC721: can't mint burned token");

622: require(_isApproved(msg.sender, tokenId), "ERC721: not approved sender");

624: require(SourceERC721().beforeTransfer(from, to, tokenId, data));

628: require(SourceERC721().afterTransfer(from, to, tokenId, data));

639: require(wallet != address(0), "ERC721: zero address");

689: require(tokenOwner != address(0), "ERC721: token does not exist");

700: require(index < _allTokens.length, "ERC721: index out of bounds");

729: require(index < balanceOf(wallet), "ERC721: index out of bounds");

757: require(_isContract(_operator), "ERC721: operator not contract");

759: require(SourceERC721().beforeOnERC721Received(_operator, _from, address(this), _tokenId, _data));

762: require(tokenOwner == address(this), "ERC721: contract not token owner");

767: require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));

815: require(tokenId > 0, "ERC721: token id cannot be zero");

816: require(to != address(0), "ERC721: minting to burn address");

817: require(!_exists(tokenId), "ERC721: token already exists");

818: require(!_burnedTokens[tokenId], "ERC721: token has been burned");

869: require(_tokenOwner[tokenId] == from, "ERC721: token not owned");

870: require(to != address(0), "ERC721: use burn instead");

906: require(_exists(tokenId), "ERC721: token does not exist");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```solidity
File: /contracts/enforcer/HolographERC20.sol

192: require(msg.sender == _holograph().getBridge(), "ERC20: bridge only call");

204: require(msg.sender == sourceContract, "ERC20: source only call");

219: require(!_isInitialized(), "ERC20: already initialized");

241: require(sourceContract.init(initCode) == InitializableInterface.init.selector, "ERC20: could not init source");

328: require(SourceERC20().beforeApprove(msg.sender, spender, amount));

332: require(SourceERC20().afterApprove(msg.sender, spender, amount));

339: require(SourceERC20().beforeBurn(msg.sender, amount));

343: require(SourceERC20().afterBurn(msg.sender, amount));

349: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

354: require(SourceERC20().beforeBurn(account, amount));

358: require(SourceERC20().afterBurn(account, amount));

365: require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");

371: require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));

375: require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));

387: require(SourceERC20().bridgeIn(fromChain, from, to, amount, data), "HOLOGRAPH: bridge in failed");

400: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

427: require(newAllowance >= currentAllowance, "ERC20: increased above max value");

430: require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));

434: require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));

445: require(_isContract(account), "ERC20: operator not contract");

447: require(SourceERC20().beforeOnERC20Received(account, sender, address(this), amount, data));

450: require(balance >= amount, "ERC20: balance check failed");

455: require(SourceERC20().afterOnERC20Received(account, sender, address(this), amount, data));

469: require(block.timestamp <= deadline, "ERC20: expired deadline");

482: require(signer == account, "ERC20: invalid signature");

484: require(SourceERC20().beforeApprove(account, spender, amount));

488: require(SourceERC20().afterApprove(account, spender, amount));

502: require(SourceERC20().beforeSafeTransfer(msg.sender, recipient, amount, data));

505: require(_checkOnERC20Received(msg.sender, recipient, amount, data), "ERC20: non ERC20Receiver");

507: require(SourceERC20().afterSafeTransfer(msg.sender, recipient, amount, data));

529: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

536: require(SourceERC20().beforeSafeTransfer(account, recipient, amount, data));

539: require(_checkOnERC20Received(account, recipient, amount, data), "ERC20: non ERC20Receiver");

541: require(SourceERC20().afterSafeTransfer(account, recipient, amount, data));

582: require(SourceERC20().beforeTransfer(msg.sender, recipient, amount));

586: require(SourceERC20().afterTransfer(msg.sender, recipient, amount));

599: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

606: require(SourceERC20().beforeTransfer(account, recipient, amount));

610: require(SourceERC20().afterTransfer(account, recipient, amount));

620: require(account != address(0), "ERC20: account is zero address");

621:  require(spender != address(0), "ERC20: spender is zero address");

627: require(account != address(0), "ERC20: account is zero address");

629: require(accountBalance >= amount, "ERC20: amount exceeds balance");

645: require(erc165support, "ERC20: no ERC165 support");

684: require(to != address(0), "ERC20: minting to burn address");

695: require(account != address(0), "ERC20: account is zero address");

696: require(recipient != address(0), "ERC20: recipient is zero address");

698: require(accountBalance >= amount, "ERC20: amount exceeds balance");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

```solidity
File: /contracts/abstract/ERC721H.sol

117: require(msg.sender == holographer(), "ERC721: holographer only");

123: require(msgSender() == _getOwner(), "ERC721: owner only function");

125: require(msg.sender == _getOwner(), "ERC721: owner only function");

147: require(!_isInitialized(), "ERC721: already initialized");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol

```solidity
File: /contracts/abstract/ERC20H.sol

117: require(msg.sender == holographer(), "ERC20: holographer only");

123: require(msgSender() == _getOwner(), "ERC20: owner only function");

125: require(msg.sender == _getOwner(), "ERC20: owner only function");

147: require(!_isInitialized(), "ERC20: already initialized");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol

### G-03 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **10** instances of this issue:_

```solidity
File: /contracts/HolographOperator.sol

378: _bondedAmounts[job.operator] -= amount;

382: _bondedAmounts[msg.sender] += amount;

834: _bondedAmounts[operator] += amount;

1175: position -= threshold;

1177: current += (current / _operatorThresholdDivisor) * (position / _operatorThresholdStep);
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/HolographFactory.sol

328: v += 27;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```solidity
File: /contracts/enforcer/HolographERC20.sol

633: _totalSupply -= amount;

685: _totalSupply += amount;

686: _balances[to] += amount;

702: _balances[recipient] += amount;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

### G-04 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **18** instances of this issue:_

```solidity
File: /contracts/HolographOperator.sol

354: require(gasPrice >= tx.gasprice, "HOLOGRAPH: gas spike detected");

386: if (_bondedAmounts[job.operator] >= amount) {

728: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

739: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

756: require(_operatorPods.length >= pod, "HOLOGRAPH: pod does not exist");

863: require(current <= amount, "HOLOGRAPH: bond amount too small");

871: for (uint256 i = _operatorPods.length; i <= pod; i++) {

1169: if (pod >= _operatorPods.length) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/enforcer/HolographERC20.sol

349: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

365: require(currentAllowance >= subtractedValue, "ERC20: decreased below zero");

400: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

427: require(newAllowance >= currentAllowance, "ERC20: increased above max value");

450: require(balance >= amount, "ERC20: balance check failed");

469: require(block.timestamp <= deadline, "ERC20: expired deadline");

529: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

599: require(currentAllowance >= amount, "ERC20: amount exceeds allowance");

629: require(accountBalance >= amount, "ERC20: amount exceeds balance");

689: require(accountBalance >= amount, "ERC20: amount exceeds balance");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

### G-05 Splitting `require()` statements that use `&&` saves gas

Instead of using && on single require check using two require checks can save gas

_There are **4** instances of this issue:_

```solidity
File: /contracts/HolographOperator.sol

857: require(_bondedOperators[operator] == 0 && _bondedAmounts[operator] == 0, "HOLOGRAPH: operator is bonded");
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/enforcer/HolographERC721.sol

263: require(success && selector == InitializableInterface.init.selector, "ERC721: coud not init PA1D");

465: (ERC165(to).supportsInterface(ERC165.supportsInterface.selector) &&

466: ERC165(to).supportsInterface(ERC721TokenReceiver.onERC721Received.selector) &&
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

### G-06 The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

This change would save up to 6 gas per instance/loop.

_There are **18** instances of this issue:_

```solidity
File: /contracts/HolographOperator.sol

781: for (uint256 i = 0; i < length; i++) {

871: for (uint256 i = _operatorPods.length; i <= pod; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/enforcer/PA1D.sol

307: for (uint256 i = 0; i < length; i++) {

323: for (uint256 i = 0; i < length; i++) {

340: for (uint256 i = 0; i < length; i++) {

356: for (uint256 i = 0; i < length; i++) {

394: for (uint256 i = 0; i < length; i++) {

414: for (uint256 i = 0; i < length; i++) {

432: for (uint256 t = 0; t < tokenAddresses.length; t++) {

437: for (uint256 i = 0; i < addresses.length; i++) {

454: for (uint256 i = 0; i < addresses.length; i++) {

474: for (uint256 i = 0; i < addresses.length; i++) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```solidity
File: /contracts/enforcer/HolographERC721.sol

357: require(initialized == 0, "PA1D: already initialized");

716: for (uint256 i = 0; i < length; i++) {

779: _ownedTokensCount[to]++;

842: _ownedTokensCount[from]--;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```solidity
File: /contracts/enforcer/HolographERC20.sol

564: for (uint256 i = 0; i < wallets.length; i++) {

713: _nonces[account]++;
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

### G-07 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **17** instances of this issue:_

```solidity
File: /contracts/HolographOperator.sol

285: ) external onlyAdmin {

949: function setBridge(address bridge) external onlyAdmin {

969: function setHolograph(address holograph) external onlyAdmin {

989: function setInterfaces(address interfaces) external onlyAdmin {

1009: function setMessagingModule(address messagingModule) external onlyAdmin {

1029: function setRegistry(address registry) external onlyAdmin {

1049: function setUtilityToken(address utilityToken) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/HolographFactory.sol

280: function setHolograph(address holograph) external onlyAdmin {

300: function setRegistry(address registry) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```solidity
File: /contracts/module/LayerZeroModule.sol

320: function setBridge(address bridge) external onlyAdmin {

340: function setInterfaces(address interfaces) external onlyAdmin {

360: function setLZEndpoint(address lZEndpoint) external onlyAdmin {

380: function setOperator(address operator) external onlyAdmin {

441: function setBaseGas(uint256 baseGas) external onlyAdmin {

470: function setGasPerByte(uint256 gasPerByte) external onlyAdmin {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol

```solidity
File: /contracts/enforcer/PA1D.sol

471: function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

533: ) public onlyOwner {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

### G-08 Not `using` the named `return` variables when a `function` returns wastes deployment gas

_There are **7** instances of this issue:_

```solidity
File: /contracts/HolographBridge.sol

301: ) external returns (string memory revertReason) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol

```solidity
File: /contracts/HolographOperator.sol

717: function getTotalPods() external view returns (uint256 totalPods) {

804: function getBondedAmount(address operator) external view returns (uint256 amount) {

814: function getBondedPod(address operator) external view returns (uint256 pod) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/HolographFactory.sol

181: ) external pure returns (bytes4 selector, bytes memory data) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```solidity
File: /contracts/module/LayerZeroModule.sol

256: ) external view returns (uint256 hlgFee, uint256 msgFee) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol

```solidity
File: /contracts/enforcer/PA1D.sol

665: function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol
