# DebtDAO

## QA Report

### L-01 Unused/empty `receive()`/`fallback()` function

If the intention is for the Ether to be used, the function should call another function, otherwise it should revert (e.g. require(msg.sender == address(weth))). Having no access control on the function means that someone may send Ether to the contract, and have no way to get anything back out, which is a loss of funds

_There are **2** instances of this issue:_

```solidity
File: /contracts/modules/spigot/Spigot.sol

227: receive() external payable {
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/spigot/Spigot.sol

```solidity
File: /contracts/modules/credit/SpigotedLine.sol

272: receive() external payable {}
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/credit/SpigotedLine.sol

### N-01 `require()`/`revert()` statements should have descriptive reason strings

_There are **22** instances of this issue:_

```solidity
File: /contracts/modules/credit/EscrowedLine.sol

64: require(escrow_.liquidate(amount, targetToken, to));

90: require(escrow.updateLine(newLine));
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/credit/EscrowedLine.sol

```solidity
File: /contracts/modules/credit/LineOfCredit.sol

112: require(uint(status) >= uint( LineLib.STATUS.ACTIVE));

259: require(interestRate.setRate(id, drate, frate));

326: require(amount <= credit.principal + credit.interestAccrued);
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/credit/LineOfCredit.sol

```solidity
File: /contracts/modules/credit/SpigotedLine.sol

62: require(defaultRevenueSplit_ <= SpigotedLineLib.MAX_SPLIT);

143: require(amount <= unusedTokens[credit.token]);

160: require(msg.sender == borrower);

239: require(msg.sender == arbiter);
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/credit/SpigotedLine.sol

```solidity
File: /contracts/utils/EscrowLib.sol

91: require(amount > 0);

105: require(msg.sender == ILineOfCredit(self.line).arbiter());

161: require(amount > 0);

198: require(amount > 0);

216: require(msg.sender == self.line);
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/EscrowLib.sol

```solidity
File: /contracts/utils/SpigotLib.sol

96: require(LineLib.sendOutTokenOrETH(token, self.treasury, claimed - escrowedAmount));

128: require(revenueContract != address(this));

130: require(self.settings[revenueContract].transferOwnerFunction == bytes4(0));

155: require(success);

180: require(newOwner != address(0));

189: require(newOperator != address(0));

201: require(newTreasury != address(0));
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/SpigotLib.sol

```solidity
File: /contracts/utils/SpigotedLineLib.sol

147: require(ISpigot(spigot).updateOwner(newLine));
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/SpigotedLineLib.sol

### N-02 Not using the named `return` variables anywhere in the `function` is confusing

Consider changing the variable to be an unnamed one

_There are **2** instances of this issue:_

```solidity
File: /contracts/modules/spigot/Spigot.sol

72: returns (uint256 claimed)

88: returns (uint256 claimed)
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/spigot/Spigot.sol

### N-03 Non-floating `pragma` should be used

_There are **5** instances of this issue:_

```solidity
File: /contracts/modules/credit/LineOfCredit.sol

File: /contracts/modules/credit/SecuredLine.sol

File: /contracts/modules/credit/SpigotedLine.sol

File: /contracts/modules/oracle/Oracle.sol

File: /contracts/modules/interest-rate/InterestRateCredit.sol
```

### N-04 Open TODOs

_There are **2** instances of this issue:_

```solidity
File: /contracts/modules/factories/LineFactory.sol

140: // TODO: test

145: // TODO: test
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/factories/LineFactory.sol

### N-05 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

_There are **2** instances of this issue:_

```solidity
File: /contracts/utils/CreditLib.sol

117: return price <= 0 ? 0 : (amount * uint(price)) / (1 * 10 ** decimals);
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/CreditLib.sol

```solidity
File: /contracts/utils/EscrowLib.sol

42: uint256 _numerator = collateralValue * 10**5; // scale to 4 decimals
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/EscrowLib.sol

### N-06 Constant redefined elsewhere

_There are **3** instances of this issue:_

```solidity
File: /contracts/utils/SpigotedLineLib.sol

13: uint8 constant MAX_SPLIT = 100;
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/SpigotedLineLib.sol

```solidity
File: /contracts/modules/factories/LineFactory.sol

13: uint8 constant MAX_SPLIT = 100; // max % to take
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/factories/LineFactory.sol

```solidity
File: /contracts/utils/SpigotLib.sol

25: uint8 constant MAX_SPLIT = 100;
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/SpigotLib.sol

### N-07 Event is missing `indexed` fields

Each `event` should use three `indexed` fields if there are three or more fields

_There are **1** instances of this issue:_

```solidity
File: /contracts/utils/MutualConsent.sol

21: event MutualConsentRegistered(
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/MutualConsent.sol
