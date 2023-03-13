# RabbitHole

## Gas Optimizations Report

### G-01 Use three loops to get token count

There are 2 loops in `getOwnedTokenIdsOfQuest` function to get the filtered tokens and 1 in `claim` function to get `redeemableTokenCount`. These 3 loops should definitely be combined. An example is to add second return value (redeemableTokenCount) in `getOwnedTokenIdsOfQuest` function.

```solidity
File: /contracts/Quest.sol

96: function claim() public virtual onlyQuestActive {
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/Quest.sol

### G-02 Use `bool` param instead of keccak256 comparison

Instead of comparing keccak256 hashes, a single boolean variable can be added. It's a different approach that is way more gas effecient and should be considered.

```solidity
File: /contracts/QuestFactory.sol

61: function createQuest(
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/QuestFactory.sol

### G-03 Don't compare boolean expressions to boolean literals

Use if(x)/if(!x) instead of if(x == true)/if(x == false).

```solidity
File: /contracts/QuestFactory.sol

73: if (rewardAllowlist[rewardTokenAddress_] == false) revert RewardNotAllowed();

221: if (quests[questId_].addressMinted[msg.sender] == true) revert AddressAlreadyMinted();
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/QuestFactory.sol

```solidity
File: /contracts/Quest.sol

136: return claimedList[tokenId_] == true;
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/Quest.sol

### G-04 `public` functions not called by the contract should be declared `external` instead

```solidity
File: /contracts/QuestFactory.sol

61: function createQuest(

142: function changeCreateQuestRole(address account_, bool canCreateQuest_) public onlyOwner {

159: function setClaimSignerAddress(address claimSignerAddress_) public onlyOwner {

172: function setRabbitHoleReceiptContract(address rabbitholeReceiptContract_) public onlyOwner {

179: function setRewardAllowlistAddress(address rewardAddress_, bool allowed_) public onlyOwner {

219: function mintReceipt(string memory questId_, bytes32 hash_, bytes memory signature_) public {
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/QuestFactory.sol

```solidity
File: /contracts/Quest.sol

50: function start() public virtual onlyOwner {

57: function pause() public onlyOwner onlyStarted {

63: function unPause() public onlyOwner onlyStarted {

96: function claim() public virtual onlyQuestActive {

140: function getRewardAmount() public view returns (uint256) {

145: function getRewardToken() public view returns (address) {
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/Quest.sol

```solidity
File: /contracts/RabbitHoleTickets.sol

54: function setTicketRenderer(address ticketRenderer_) public onlyOwner {

60: function setRoyaltyRecipient(address royaltyRecipient_) public onlyOwner {

66: function setRoyaltyFee(uint256 royaltyFee_) public onlyOwner {

73: function setMinterAddress(address minterAddress_) public onlyOwner {

83: function mint(address to_, uint256 id_, uint256 amount_, bytes memory data_) public onlyMinter {

92: function mintBatch(

102: function uri(uint tokenId_) public view virtual override(ERC1155Upgradeable) returns (string memory) {
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/RabbitHoleTickets.sol

```solidity
File: /contracts/Erc20Quest.sol

102: function withdrawFee() public onlyAdminWithdrawAfterEnd {
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/Erc20Quest.sol
