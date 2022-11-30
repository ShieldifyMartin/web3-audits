# InverseFinance

## QA Report

### L-01 Zero-`address` checks are missing

_There are **12** instances of this issue:_

```solidity
File: /src/Market.sol

130: function setGov(address _gov) public onlyGov { gov = _gov; }

136: function setLender(address _lender) public onlyGov { lender = _lender; }

142: function setPauseGuardian(address _pauseGuardian) public onlyGov { pauseGuardian = _pauseGuardian; }
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```solidity
File: /src/DBR.sol

53: function setPendingOperator(address newOperator_) public onlyOperator {

81: function addMinter(address minter_) public onlyOperator {

99: function addMarket(address market_) public onlyOperator {
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```solidity
File: /src/BorrowController.sol

26: function setOperator(address _operator) public onlyOperator { operator = _operator; }

32: function allow(address allowedContract) public onlyOperator { contractAllowlist[allowedContract] = true; }

38: function deny(address deniedContract) public onlyOperator { contractAllowlist[deniedContract] = false; }
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/BorrowController.sol

```solidity
File: /src/Oracle.sol

44: function setPendingOperator(address newOperator_) public onlyOperator { pendingOperator = newOperator_; }

53: function setFeed(address token, IChainlinkFeed feed, uint8 tokenDecimals) public onlyOperator { feeds[token] = FeedData(feed, tokenDecimals); }

61: function setFixedPrice(address token, uint price) public onlyOperator { fixedPrices[token] = price; }
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

### L-02 Use of floating `pragma`

_There are **8** instances of this issue:_

```solidity
File: /src/Market.sol

File: /src/DBR.sol

File: /src/BorrowController.sol

File: /src/Oracle.sol

File: /src/Fed.sol

File: /src/escrows/SimpleERC20Escrow.sol

File: /src/escrows/INVEscrow.sol

File: /src/escrows/GovTokenEscrow.sol
```

### L-03 Return values of `approve()` not checked

Not all IERC20 implementations revert() when there’s a failure in approve(). The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything.

_There are **1** instances of this issue:_

```solidity
File: /src/escrows/INVEscrow.sol

50: _token.approve(address(xINV), type(uint).max);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/escrows/INVEscrow.sol

### N-01 `public` functions not called by the contract should be declared `external` instead

_There are **72** instances of this issue:_

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

### N-02 `constants` should be defined rather than using magic numbers

_There are **23** instances of this issue:_

```solidity
File: /src/Market.sol

74: require(_collateralFactorBps < 10000, "Invalid collateral factor");

75: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps < 10000, "Invalid liquidation incentive");

76: require(_replenishmentIncentiveBps < 10000, "Replenishment incentive must be less than 100%");

150: require(_collateralFactorBps < 10000, "Invalid collateral factor");

162: require(_liquidationFactorBps > 0 && _liquidationFactorBps <= 10000, "Invalid liquidation factor");

173: require(_replenishmentIncentiveBps > 0 && _replenishmentIncentiveBps < 10000, "Invalid replenishment incentive");

184: require(_liquidationIncentiveBps > 0 && _liquidationIncentiveBps + liquidationFeeBps < 10000, "Invalid liquidation incentive");

195: require(_liquidationFeeBps > 0 && _liquidationFeeBps + liquidationIncentiveBps < 10000, "Invalid liquidation fee");

336: return collateralValue * collateralFactorBps / 10000;

346: return collateralValue * collateralFactorBps / 10000;

360: uint minimumCollateral = debt * 1 ether / oracle.getPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

377: uint minimumCollateral = debt * 1 ether / oracle.viewPrice(address(collateral), collateralFactorBps) * 10000 / collateralFactorBps;

563: uint replenishmentCost = amount * dbr.replenishmentPriceBps() / 10000;

564: uint replenisherReward = replenishmentCost * replenishmentIncentiveBps / 10000;

583: return debt * liquidationFactorBps / 10000;

595: require(repaidDebt <= debt * liquidationFactorBps / 10000, "Exceeded liquidation factor");

598: liquidatorReward += liquidatorReward * liquidationIncentiveBps / 10000;

606: uint liquidationFee = repaidDebt * 1 ether / price * liquidationFeeBps / 10000;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Market.sol

```solidity
File: /src/DBR.sol

330: uint replenishmentCost = amount * replenishmentPriceBps / 10000;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/DBR.sol

```solidity
File: /src/Oracle.sol

95: uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;

98: uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;

134: uint newBorrowingPower = normalizedPrice * collateralFactorBps / 10000;

137: uint dampenedPrice = twoDayLow * 10000 / collateralFactorBps;
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol

### N-03 Use scientific notation (e.g. 1e18) rather than exponentiation (e.g. 10`**`18)

_There are **2** instances of this issue:_

```solidity
File: /src/Oracle.sol

88: uint normalizedPrice = price * (10 ** decimals);

122: uint normalizedPrice = price * (10 ** decimals);
```

https://github.com/code-423n4/2022-10-inverse/blob/main/src/Oracle.sol
