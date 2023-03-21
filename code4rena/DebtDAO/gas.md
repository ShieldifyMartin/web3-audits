# DebtDAO

## Gas Optimizations Report

### G-01 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **11** instances of this issue:_

```solidity
File: /contracts/modules/credit/LineOfCredit.sol

112: require(uint(status) >= uint( LineLib.STATUS.ACTIVE));

132: if (block.timestamp >= deadline && count > 0) {

326: require(amount <= credit.principal + credit.interestAccrued);

520: for (uint256 i; i <= lastSpot; ++i) {
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/credit/LineOfCredit.sol

```solidity
File: /contracts/modules/credit/SpigotedLine.sol

62: require(defaultRevenueSplit_ <= SpigotedLineLib.MAX_SPLIT);

143: require(amount <= unusedTokens[credit.token]);
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/credit/SpigotedLine.sol

```solidity
File: /contracts/utils/CreditLib.sol

117: return price <= 0 ? 0 : (amount * uint(price)) / (1 * 10 ** decimals);

136: if(price <= 0 ) { revert NoTokenPrice(); }

176: if (amount <= credit.interestAccrued) {
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/CreditLib.sol

```solidity
File: /contracts/utils/CreditListLib.sol

42: if(len <= 1) return true; // already ordered
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/CreditListLib.sol

```solidity
File: /contracts/utils/EscrowLib.sol

127: if (price <= 0) {
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/EscrowLib.sol

### G-02 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **20** instances of this issue:_

```solidity
File: /contracts/modules/credit/LineOfCredit.sol

192: principal += _p;

193: interest += _i;

276: credit.deposit += amount;

351: credit.principal += amount;
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/credit/LineOfCredit.sol

```solidity
File: /contracts/modules/credit/SpigotedLine.sol

122: unusedTokens[credit.token] -= repaid - newTokens;

125: unusedTokens[credit.token] += newTokens - repaid;

144: unusedTokens[credit.token] -= amount;

172: unusedTokens[targetToken] += newTokens;
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/credit/SpigotedLine.sol

```solidity
File: /contracts/utils/CreditLib.sol

177: credit.interestAccrued -= amount;

178: credit.interestRepaid += amount;

186: credit.principal -= principalPayment;

187: credit.interestRepaid += interest;

216: amount -= interest;

218: credit.deposit -= amount;

227: credit.interestRepaid -= amount;

258: credit.interestAccrued += accruedToken;
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/CreditLib.sol

```solidity
File: /contracts/utils/EscrowLib.sol

75: collateralValue += CreditLib.calculateValue(

96: self.deposited[token].amount += amount;

164: self.deposited[token].amount -= amount;

202: self.deposited[token].amount -= amount;
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/EscrowLib.sol

### G-03 State variables can be packed into fewer storage slots

_There are **2** instances of this issue:_

```solidity
File: /contracts/modules/credit/LineOfCredit.sol

21: uint256 public immutable deadline;

31: uint256 private count; // amount of open credit lines on a Line of Credit facility. ids.length includes null items
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/credit/LineOfCredit.sol

### G-04 Duplicated `require()`/`revert()` checks should be refactored to a modifier or function

_There are **5** instances of this issue:_

```solidity
File: /contracts/modules/credit/LineOfCredit.sol

241: require(interestRate.setRate(id, drate, frate));

259: require(interestRate.setRate(id, drate, frate));
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/credit/LineOfCredit.sol

```solidity
File: /contracts/utils/EscrowLib.sol

91: require(amount > 0);

161: require(amount > 0);

198: require(amount > 0);
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/utils/EscrowLib.sol

### G-05 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **2** instances of this issue:_

```solidity
File: /contracts/modules/interest-rate/InterestRateCredit.sol

38: ) external override onlyLineContract returns (uint256) {

78: ) external onlyLineContract returns (bool) {
```

https://github.com/debtdao/Line-of-Credit/blob/audit/code4rena-2022-11-03/contracts/modules/interest-rate/InterestRateCredit.sol

### G-06 Use custom errors rather than `revert()`/`require()` strings to save gas

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
