# Holograph

## QA Report

### L-01 Zero-`address` checks are missing

_There are **8** instances of this issue:_

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
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol

```solidity
File: /contracts/abstract/ERC721H.sol

203: function _setOwner(address ownerAddress) internal {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC721H.sol

```solidity
File: /contracts/abstract/ERC20H.sol

203: function _setOwner(address ownerAddress) internal {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/abstract/ERC20H.sol

### L-02 Modifiers shouldn’t update state

_There are **2** instances of this issue:_

```solidity
File: /contracts/enforcer/HolographERC721.sol

219: modifier onlySource() {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```solidity
File: /contracts/enforcer/HolographERC20.sol

199: modifier onlySource() {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

### N-01 `require()`/`revert()` statements should have descriptive reason strings

_There are **45** instances of this issue:_

```solidity
File: /contracts/HolographBridge.sol

233: require(doNotRevert, "HOLOGRAPH: reverted");

578: revert();

585: revert();
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographBridge.sol

```solidity
File: /contracts/HolographOperator.sol

474: revert(0, 0)

562: revert(0, returndatasize())

676: revert(0, returndatasize())

1215: revert();
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/HolographFactory.sol

341: revert();

348: revert();
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographFactory.sol

```solidity
File: /contracts/module/LayerZeroModule.sol

417: revert();

424: revert();
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol

```solidity
File: /contracts/enforcer/HolographERC721.sol

373: require(SourceERC721().beforeApprove(tokenOwner, to, tokenId));

378: require(SourceERC721().afterApprove(tokenOwner, to, tokenId));

391: require(SourceERC721().beforeBurn(wallet, tokenId));

395: require(SourceERC721().afterBurn(wallet, tokenId));

460: require(SourceERC721().beforeSafeTransfer(from, to, tokenId, data));

473: require(SourceERC721().afterSafeTransfer(from, to, tokenId, data));

486: require(SourceERC721().beforeApprovalAll(to, approved));

491: require(SourceERC721().afterApprovalAll(to, approved));

624: require(SourceERC721().beforeTransfer(from, to, tokenId, data));

628: require(SourceERC721().afterTransfer(from, to, tokenId, data));

759: require(SourceERC721().beforeOnERC721Received(_operator, _from, address(this), _tokenId, _data));

767: require(SourceERC721().afterOnERC721Received(_operator, _from, address(this), _tokenId, _data));
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```solidity
File: /contracts/enforcer/HolographERC20.sol

328: require(SourceERC20().beforeApprove(msg.sender, spender, amount));

332: require(SourceERC20().afterApprove(msg.sender, spender, amount));

339: require(SourceERC20().beforeBurn(msg.sender, amount));

343: require(SourceERC20().afterBurn(msg.sender, amount));

354: require(SourceERC20().beforeBurn(account, amount));

358: require(SourceERC20().afterBurn(account, amount));

371: require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));

375: require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));

430: require(SourceERC20().beforeApprove(msg.sender, spender, newAllowance));

434: require(SourceERC20().afterApprove(msg.sender, spender, newAllowance));

447: require(SourceERC20().beforeOnERC20Received(account, sender, address(this), amount, data));

455: require(SourceERC20().afterOnERC20Received(account, sender, address(this), amount, data));

484: require(SourceERC20().beforeApprove(account, spender, amount));

488: require(SourceERC20().afterApprove(account, spender, amount));

502: require(SourceERC20().beforeSafeTransfer(msg.sender, recipient, amount, data));

507: require(SourceERC20().afterSafeTransfer(msg.sender, recipient, amount, data));

536: require(SourceERC20().beforeSafeTransfer(account, recipient, amount, data));

541: require(SourceERC20().afterSafeTransfer(account, recipient, amount, data));

582: require(SourceERC20().beforeTransfer(msg.sender, recipient, amount));

586: require(SourceERC20().afterTransfer(msg.sender, recipient, amount));

606: require(SourceERC20().beforeTransfer(account, recipient, amount));

610: require(SourceERC20().afterTransfer(account, recipient, amount));
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol

### N-02 Not using the named `return` variables anywhere in the `function` is confusing

Consider changing the variable to be an unnamed one

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

### N-03 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. `10**18`)

_There are **4** instances of this issue:_

```solidity
File: /contracts/HolographOperator.sol

256: _baseBondAmount = 100 * (10**18); // one single token unit * 100

1172: uint256 threshold = _operatorThreshold / (2**pod);
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/HolographOperator.sol

```solidity
File: /contracts/module/LayerZeroModule.sol

274: return (((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10), nativeFee);

293: return ((gasPrice * (gasLimit + (gasLimit / 10))) * dstPriceRatio) / (10**10);
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/module/LayerZeroModule.sol

### N-04 Event is missing `indexed` fields

Each `event` should use three `indexed` fields if there are three or more fields

_There are **1** instances of this issue:_

```solidity
File: /contracts/enforcer/PA1D.sol

153: event SecondarySaleFees(uint256 tokenId, address[] recipients, uint256[] bps);
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

### N-05 `public` functions not called by the contract should be declared `external` instead

_There are **33** instances of this issue:_

```solidity
File: /contracts/enforcer/PA1D.sol

471: function configurePayouts(address payable[] memory addresses, uint256[] memory bps) public onlyOwner {

488: function getPayoutInfo() public view returns (address payable[] memory addresses, uint256[] memory bps) {

497: function getEthPayout() public {

507: function getTokenPayout(address tokenAddress) public {

517: function getTokensPayout(address[] memory tokenAddresses) public {

549: function royaltyInfo(uint256 tokenId, uint256 value) public view returns (address, uint256) {

558: function getFeeBps(uint256 tokenId) public view returns (uint256[] memory) {

569: function getFeeRecipients(uint256 tokenId) public view returns (address payable[] memory) {

590: function getRoyalties(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {

604: function getFees(uint256 tokenId) public view returns (address payable[] memory, uint256[] memory) {

621: function tokenCreator(

634: function calculateRoyaltyFee(

649: function marketContract() public view returns (address) {

655: function tokenCreators(uint256 tokenId) public view returns (address) {

665: function bidSharesForToken(uint256 tokenId) public view returns (ZoraBidShares memory bidShares) {

683: function getTokenAddress(string memory tokenName) public view returns (address) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/PA1D.sol

```solidity
File: /contracts/enforcer/HolographERC721.sol

643: function burned(uint256 tokenId) public view returns (bool) {

656: function exists(uint256 tokenId) public view returns (bool) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC721.sol

```solidity
File: /contracts/enforcer/HolographERC20.sol

273: function decimals() public view returns (uint8) {

297: function allowance(address account, address spender) public view returns (uint256) {

310: function name() public view returns (string memory) {

314: function nonces(address account) public view returns (uint256) {

318: function symbol() public view returns (string memory) {

322: function totalSupply() public view returns (uint256) {

326: function approve(address spender, uint256 amount) public returns (bool) {

337: function burn(uint256 amount) public {

347: function burnFrom(address account, uint256 amount) public returns (bool) {

363: function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {

420: function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {

460: function permit(

492: function safeTransfer(address recipient, uint256 amount) public returns (bool) {

512: function safeTransferFrom(

740: function owner() public view override returns (address) {
```

https://github.com/code-423n4/2022-10-holograph/blob/main/contracts/enforcer/HolographERC20.sol
