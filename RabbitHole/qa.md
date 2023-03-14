# RabbitHole

## QA Report

### L-01 `onlyQuestActive` modifier is missing crucial checks

`onlyQuestActive` modifier doesn't check if contract is had been paused or had ended. It these cases Quest is certainly not active but the logic in the function with such modifiers get executed. This check is however added in `claim` function but is absolutely not in place and could easily be forgotten, so this is at least Low severity.

```solidity
File: /contracts/Quest.sol

88: modifier onlyQuestActive() {
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/Quest.sol

### L-02 `emit` function called early

```solidity
File: /contracts/QuestFactory.sol

87: emit QuestCreated(

118: emit QuestCreated(

227: emit ReceiptMinted(msg.sender, questId_);
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/QuestFactory.sol

### L-03 Events not emitted for important state changes

```solidity
File: /contracts/RabbitHoleReceipt.sol

98: function mint(address to_, string memory questId_) public onlyMinter {
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/RabbitHoleReceipt.sol

### L-04 `_safeMint()` should be used rather than `_mint()` wherever possible

`safeMint` does an unsafe external call to the recipient, so there is a reentrency attack vector. Make sure to add 'nonReentrant' modifier.

```solidity
File: /contracts/QuestFactory.sol

228: rabbitholeReceiptContract.mint(msg.sender, questId_);
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/QuestFactory.sol

```solidity
File: /contracts/RabbitHoleTickets.sol

84: _mint(to_, id_, amount_, data_);
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/RabbitHoleTickets.sol

### L-05 Missing address validation

`startTime_` and `endTime_` should at least be compared

```solidity
File: /contracts/QuestFactory.sol

61: function createQuest(
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/QuestFactory.sol

### L-06 Using multi-purpose variable

From logical perspective it's always a bad idea. Every variable should have single purpose because otherwise it can easily lead to wrong input data. This combined with zero validation checks for this specific param is vector for `Erc20Quest` with wrong data.

```solidity
File: /contracts/QuestFactory.sol

66: uint256 rewardAmountOrTokenId_,
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/QuestFactory.sol

### N-01 Non-floating `pragma` should be used

```solidity
File: /contracts/QuestFactory.sol

File: /contracts/RabbitHoleReceipt.sol

File: /contracts/interfaces/IQuest.sol

File: /contracts/interfaces/IQuestFactory.sol

File: /contracts/Quest.sol

File: /contracts/RabbitHoleTickets.sol

File: /contracts/TicketRenderer.sol

File: /contracts/ReceiptRenderer.sol

File: /contracts/Erc20Quest.sol

File: /contracts/Erc1155Quest.sol
```

### N-02 Recommended functions order in solidity should be followed to improve code quality

It is recommended to stricly follow solidity best code-style practices.

```solidity
File: /contracts/QuestFactory.sol

File: /contracts/RabbitHoleReceipt.sol

File: /contracts/Quest.sol

File: /contracts/Erc20Quest.sol

File: /contracts/Erc1155Quest.sol
```

### N-03 Typos

```solidity
File: /contracts/QuestFactory.sol

176: /// @dev set or remave a contract address to be used as a reward
```

### N-04 Open TODOs

```solidity
File: /contracts/interfaces/IQuest.sol
```

### N-05 Unused imports

```solidity
File: /contracts/Quest.sol

7: import {ECDSA} from '@openzeppelin/contracts/utils/cryptography/ECDSA.sol';
```

https://github.com/rabbitholegg/quest-protocol/blob/8c4c1f71221570b14a0479c216583342bd652d8d/contracts/Quest.sol
