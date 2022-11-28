# InverseFinance

## Gas Optimizations Report

### G-01 Using `bools` for storage incurs overhead

Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas) for the extra SLOAD, and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past.

_There are **2** instances of this issue:_

```solidity
File: /src/Market.sol

52: bool immutable callOnDepositCallback;

53: bool public borrowPaused;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

### G-02 Duplicated `require()`/`revert()` checks should be refactored to a `modifier` or `function`

_There are **12** instances of this issue:_

```solidity
File: /src/Market.sol

74: require(_collateralFactorBps < 10000, "Invalid collateral factor");

150: require(_collateralFactorBps < 10000, "Invalid collateral factor");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```solidity
File: /src/DBR.sol

195: require(balanceOf(from) >= amount, "Insufficient balance");

373: require(balanceOf(from) >= amount, "Insufficient balance");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```solidity
File: /src/Oracle.sol

83: require(price > 0, "Invalid feed price");

117: require(price > 0, "Invalid feed price");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```solidity
File: /src/Fed.sol

49: require(msg.sender == gov, "ONLY GOV");

58: require(msg.sender == gov, "ONLY GOV");

67: require(msg.sender == gov, "ONLY GOV");

76: require(msg.sender == chair, "ONLY CHAIR");

87: require(msg.sender == chair, "ONLY CHAIR");

104: require(msg.sender == chair, "ONLY CHAIR");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

### G-03 `require()`/`revert()` strings longer than 32 bytes cost extra gas

Each extra memory word of bytes past the original 32 incurs an MSTORE which costs 3 gas

_There are **9** instances of this issue:_

```solidity
File: /src/Market.sol

75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

76: require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

93: require(msg.sender == gov, "Only gov can call this function");

173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

214: require(msg.sender == pauseGuardian || msg.sender == gov, "Only pause guardian or governance can pause");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```solidity
File: /src/DBR.sol

63: require(newReplenishmentPriceBps_ > 0, "replenishment price must be over 0");

314: require(markets[msg.sender], "Only markets can call onRepay");

326: require(markets[msg.sender], "Only markets can call onForceReplenish");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

### G-04 Replace `x <= y` with `x < y + 1`, and `x >= y` with `x > y - 1`

In the EVM, there is no opcode for >= or <=. When using greater than or equal, two operations are performed: > and =. Using strict comparison operators hence saves gas

_There are **21** instances of this issue:_

```solidity
File: /src/Market.sol

162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

361: if(collateralBalance <= minimumCollateral) return 0;

378: if(collateralBalance <= minimumCollateral) return 0;

396: require(credit >= debts[borrower], "Exceeded credit limit");

423: require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

462: require(limit >= amount, "Insufficient withdrawal limit");

487: require(deadline >= block.timestamp, "DEADLINE_EXPIRED");

533: require(debt >= amount, "Insufficient debt");

562: require(deficit >= amount, "Amount > deficit");

567: require(collateralValue >= debts[user], "Exceeded collateral value");

582: if(credit >= debt) return 0;

595: require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");

607: if(escrow.balance() >= liquidationFee) {
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```solidity
File: /src/DBR.sol

171: require(balanceOf(msg.sender) >= amount, "Insufficient balance");

195: require(balanceOf(from) >= amount, "Insufficient balance");

224: require(deadline >= block.timestamp, "PERMIT_DEADLINE_EXPIRED");

329: require(deficit >= amount, "Amount > deficit");

373: require(balanceOf(from) >= amount, "Insufficient balance");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```solidity
File: /src/Fed.sol

93: require(globalSupply <= supplyCeiling);

107: require(amount <= supply, "AMOUNT TOO BIG"); // can't burn profits

123: if(supply >= marketValue) return 0;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

### G-05 `<x> += <y>` costs more gas than `<x> = <x> + <y>` for state variables

_There are **26** instances of this issue:_

```solidity
File: /src/Market.sol

395: debts[borrower] += amount;

397: totalDebt += amount;

565: debts[user] += replenishmentCost;

568: totalDebt += replenishmentCost;

598: liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;

534: debts[user] -= amount;

535: totalDebt -= amount;

599: debts[user] -= repaidDebt;

600: totalDebt -= repaidDebt;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```solidity
File: /src/DBR.sol

172: balances[msg.sender] -= amount;

174: balances[to] += amount;

196: balances[from] -= amount;

198: balances[to] += amount;

288: dueTokensAccrued[user] += accrued;

289: totalDueTokensAccrued += accrued;

304: debts[user] += additionalDebt;

316: debts[user] -= repaidDebt;

332: debts[user] += replenishmentCost;

360: _totalSupply += amount;

362: balances[to] += amount;

374: balances[from] -= amount;

376: _totalSupply -= amount;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```solidity
File: /src/Fed.sol

91: supplies[market] += amount;

92: globalSupply += amount;

110: supplies[market] -= amount;

111: globalSupply -= amount;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

### G-06 The usage of `++i` will cost less gas than `i++`. The same change can be applied to `i--` as well.

This change would save up to 6 gas per instance/loop.

_There are **5** instances of this issue:_

```solidity
File: /src/Market.sol

438: nonces[from]++,

502: nonces[from]++,

521: nonces[msg.sender]++;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```solidity
File: /src/DBR.sol

239: nonces[owner]++,

259: nonces[msg.sender]++;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

### G-07 Splitting `require()` statements that use `&&` saves gas

Instead of using && on single require check using two require checks can save gas

_There are **1** instances of this issue:_

```solidity
File: /src/DBR.sol

249: require(recoveredAddress != address(0) && recoveredAddress == owner, "INVALID_SIGNER");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

### G-08 Using `private` rather than `public` for constants, saves gas

If needed, the values can be read from the verified contract source code, or if there are multiple values there can be a single getter function that returns a tuple of the values of all currently-public constants. Saves 3406-3606 gas in deployment gas due to the compiler not having to create non-payable getter functions for deployment calldata, not having to store the bytes of the value outside of where it's used, and not adding another entry to the method ID table

_There are **1** instances of this issue:_

```solidity
File: /src/DBR.sol

13: uint8 public constant decimals = 18;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

### G-09 Don't compare boolean expressions to boolean literals

Use if(x)/if(!x) instead of if(x == true)/if(x == false).

_There are **1** instances of this issue:_

```solidity
File: /src/DBR.sol

350: require(minters[msg.sender] == true || msg.sender == operator, "ONLY MINTERS OR OPERATOR");
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

### G-10 Functions that are access-restricted from most users may be marked as `payable`

Marking a function as payable reduces gas cost since the compiler does not have to check whether a payment was provided or not. This change will save around 21 gas per function call.

_There are **3** instances of this issue:_

```solidity
File: /src/BorrowController.sol

26: function setOperator(address _operator) public onlyOperator { operator = _operator; }

32: function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }

38: function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol

### G-11 `public` functions not called by the contract should be declared `external` instead

When a function with a memory array is called externally, the abi.decode() step has to use a for-loop to copy each index of the calldata to the memory index. Each iteration of this for-loop costs at least 60 gas (i.e. 60 array length). Using calldata directly, obliviates the need for such a loop in the contract code and runtime execution.

_There are **65** instances of this issue:_

```solidity
File: /src/Market.sol

118: function setOracle(IOracle _oracle) public onlyGov { oracle = _oracle; }

124: function setBorrowController(IBorrowController _borrowController) public onlyGov { borrowController = _borrowController; }

130: function setGov(address _gov) public onlyGov { gov = _gov; }

136: function setLender(address _lender) public onlyGov { lender = _lender; }

142: function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }

149: function setCollateralFactorBps(uint _collateralFactorBps) public onlyGov {

161: function setLiquidationFactorBps(uint _liquidationFactorBps) public onlyGov {

172: function setReplenismentIncentiveBps(uint _replenishmentIncentiveBps) public onlyGov {

183: function setLiquidationIncentiveBps(uint _liquidationIncentiveBps) public onlyGov {

194: function setLiquidationFeeBps(uint _liquidationFeeBps) public onlyGov {

203: function recall(uint amount) public {

212: function pauseBorrows(bool _value) public {

267: function depositAndBorrow(uint amountDeposit, uint amountBorrow) public {

370: function getWithdrawalLimit(address user) public view returns (uint) {

422: function borrowOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {

486: function withdrawOnBehalf(address from, uint amount, uint deadline, uint8 v, bytes32 r, bytes32 s) public {

520: function invalidateNonce() public {

546: function repayAndWithdraw(uint repayAmount, uint withdrawAmount) public {

559: function forceReplenish(address user, uint amount) public {

578: function getLiquidatableDebt(address user) public view returns (uint) {

591: function liquidate(address user, uint repaidDebt) public {
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```solidity
File: /src/DBR.sol

53: function setPendingOperator(address newOperator_) public onlyOperator {

62: function setReplenishmentPriceBps(uint newReplenishmentPriceBps_) public onlyOperator {

70: function claimOperator() public {

81: function addMinter(address minter_) public onlyOperator {

90: function removeMinter(address minter_) public onlyOperator {

99: function addMarket(address market_) public onlyOperator {

109: function totalSupply() public view returns (uint) {

149: function signedBalanceOf(address user) public view returns (int) {

158: function approve(address spender, uint256 amount) public virtual returns (bool) {

170: function transfer(address to, uint256 amount) public virtual returns (bool) {

188: function transferFrom(

215: function permit(

258: function invalidateNonce() public {

300: function onBorrow(address user, uint additionalDebt) public {

313: function onRepay(address user, uint repaidDebt) public {

325: function onForceReplenish(address user, uint amount) public {

340: function burn(uint amount) public {

349: function mint(address to, uint amount) public {
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```solidity
File: /src/BorrowController.sol

26: function setOperator(address _operator) public onlyOperator { operator = _operator; }

32: function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }

38: function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }

46: function borrowAllowed(address msgSender, address, uint) public view returns (bool) {
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol

```solidity
File: /src/Oracle.sol

44: function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }

53: function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }

61: function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }

66: function claimOperator() public {
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

```solidity
File: /src/Fed.sol

48: function changeGov(address _gov) public {

57: function changeSupplyCeiling(uint _supplyCeiling) public {

66: function changeChair(address _chair) public {

75: function resign() public {

86: function expansion(IMarket market, uint amount) public {

103: function contraction(IMarket market, uint amount) public {

131: function takeProfit(IMarket market) public {
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Fed.sol

```solidity
File: /src/escrows/SimpleERC20Escrow.sol

25: function initialize(IERC20 _token, address) public {

36: function pay(address recipient, uint amount) public {

45: function balance() public view returns (uint) {
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/SimpleERC20Escrow.sol

```solidity
File: /src/escrows/INVEscrow.sol

44: function initialize(IERC20 _token, address _beneficiary) public {

59: function pay(address recipient, uint amount) public {

70: function balance() public view returns (uint) {

79: function onDeposit() public {

90: function delegate(address delegatee) public {
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol

```solidity
File: /src/escrows/GovTokenEscrow.sol

30: function initialize(IERC20 _token, address _beneficiary) public {

43: function pay(address recipient, uint amount) public {

52: function balance() public view returns (uint) {

66: function delegate(address delegatee) public {
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/GovTokenEscrow.sol
