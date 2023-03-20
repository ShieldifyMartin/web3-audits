# Timeswap

## QA Report

### L-01 Modifiers shouldn’t update state

Having marked these functions as `modifiers` means that they should not update state.

```solidity
66: function raiseGuard(uint256 strike, uint256 maturity) private {

71: function lowerGuard(uint256 strike, uint256 maturity) private {
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/TimeswapV2Pool.sol

### N-01 Non-floating `pragma` should be used

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/base/ERC1155Enumerable.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/interfaces/IERC1155Enumerable.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Position.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Param.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/CallbackParam.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/structs/Position.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2LiquidityToken.sol

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-token/src/TimeswapV2Token.sol

### N-02 Misleading event name

Better name would be `SetPendingOwner`. It's confusing when emit in `OwnableTwoSteps`. In front-end it could lead to wrong message or action.

```solidity
7: event SetOwner(address pendingOwner);
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/interfaces/IOwnableTwoSteps.sol

### N-03 Not using the named `return` variables anywhere in the `function` is confusing

```solidity

66: function feeGrowth(Pool storage pool) external view returns (uint256 long0FeeGrowth, uint256 long1FeeGrowth, uint256 shortFeeGrowth) {

75: function feesEarnedOf(Pool storage pool, address owner) external view returns (uint256 long0Fees, uint256 long1Fees, uint256 shortFees) {

83: function protocolFeesEarned(Pool storage pool) external view returns (uint256 long0ProtocolFees, uint256 long1ProtocolFees, uint256 shortProtocolFees) {
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-pool/src/structs/Pool.sol

```solidity
38: function totalPosition(Option storage option, uint256 strike, TimeswapV2OptionPosition position) internal view returns (uint256 balance) {

49: function positionOf(Option storage option, address owner, TimeswapV2OptionPosition position) internal view returns (uint256 balance) {
```

### N-04 Not Used Imports

```solidity
5: import {Error} from "@timeswap-labs/v2-library/src/Error.sol";

10: import {TimeswapV2OptionPosition, PositionLibrary} from "../enums/Position.sol";

11: import {TimeswapV2OptionMint, TimeswapV2OptionBurn, TimeswapV2OptionSwap, TimeswapV2OptionCollect, TransactionLibrary} from "../enums/Transaction.sol";
```

https://github.com/code-423n4/2023-01-timeswap/blob/main/packages/v2-option/src/structs/Option.sol
