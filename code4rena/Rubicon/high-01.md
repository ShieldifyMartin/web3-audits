# [H-01] The functions `RubiconMarket.batchOffer` and `RubiconMarket.batchRequote` will not work correctly due to the wrong call of the public function `SimpleMarket.offer` in them with `this` keyword.

## Impact

The main contact `RubiconMarket` inherits the contact `ExpiringMarket` and the contract `ExpiringMarket` inherits `SimpleMarket` contract.
The `SimpleMarket` contract has a public virtual function `offer` that makes a new offer and takes funds from the user(sender) into market escrow.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511-L562

```solidity
  function offer(
    uint256 pay_amt,
    ERC20 pay_gem,
    uint256 buy_amt,
    ERC20 buy_gem,
    address owner,
    address recipient
  ) public virtual can_offer synchronized returns (uint256 id) {
    require(uint128(pay_amt) == pay_amt);
    require(uint128(buy_amt) == buy_amt);
    require(pay_amt > 0);
    require(pay_gem != ERC20(address(0))); /// @dev Note, modified from: require(pay_gem != ERC20(0x0)) which compiles in 0.7.6
    require(buy_amt > 0);
    require(buy_gem != ERC20(address(0))); /// @dev Note, modified from: require(buy_gem != ERC20(0x0)) which compiles in 0.7.6
    require(pay_gem != buy_gem);

    OfferInfo memory info;
    info.pay_amt = pay_amt;
    info.pay_gem = pay_gem;
    info.buy_amt = buy_amt;
    info.buy_gem = buy_gem;
    info.recipient = recipient;
    info.owner = owner;
    info.timestamp = uint64(block.timestamp);
    id = _next_id();
    offers[id] = info;

    require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));

    emit LogItemUpdate(id);

    /// emit LogMake(
    ///     bytes32(id),
    ///     keccak256(abi.encodePacked(pay_gem, buy_gem)),
    ///     msg.sender,
    ///     pay_gem,
    ///     buy_gem,
    ///     uint128(pay_amt),
    ///     uint128(buy_amt),
    ///     uint64(block.timestamp)
    /// );

    emit emitOffer(
      bytes32(id),
      keccak256(abi.encodePacked(pay_gem, buy_gem)),
      msg.sender,
      pay_gem,
      buy_gem,
      uint128(pay_amt),
      uint128(buy_amt)
    );
  }
```

The `RubiconMarket` contract has two functions `RubiconMarket.batchOffer` and `RubiconMarket.batchRequote`.

1. The function `RubiconMarket.batchOffer` allows users to make multiple offers in a single transaction

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L887-L907

```solidity
  function batchOffer(
    uint[] calldata payAmts,
    address[] calldata payGems,
    uint[] calldata buyAmts,
    address[] calldata buyGems
  ) external {
    require(
      payAmts.length == payGems.length &&
        payAmts.length == buyAmts.length &&
        payAmts.length == buyGems.length,
      "Array lengths do not match"
    );
    for (uint i = 0; i < payAmts.length; i++) {
      this.offer(payAmts[i], ERC20(payGems[i]), buyAmts[i], ERC20(buyGems[i]));
    }
  }
```

2. The function `RubiconMarket.batchRequote` allows users to update outstanding offers to new offers in a single transaction

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917-L933

```solidity
  function batchRequote(
    uint[] calldata ids,
    uint[] calldata payAmts,
    address[] calldata payGems,
    uint[] calldata buyAmts,
    address[] calldata buyGems
  ) external {
    for (uint i = 0; i < ids.length; i++) {
      cancel(ids[i]);
      this.offer(payAmts[i], ERC20(payGems[i]), buyAmts[i], ERC20(buyGems[i]));
    }
  }
```

The problem comes from the two functions `RubiconMarket.batchOffer` and `RubiconMarket.batchRequote` which call the `SimpleMarket.offer` function within itself with the `this` keyword. Which means that the `msg.sender` in the `SimpleMarket.offer` function will always be the address of the `RubiconMarket` contract and not the address of the `user` as expected, because when you call the public function with `this` keyword, the address becomes that of the contract and not of the one who called the function.

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L538

```solidity
538: require(pay_gem.transferFrom(msg.sender, address(this), pay_amt));
```

## Proof of Concept

https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L511-L562
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L887-L907
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L917-L933
https://github.com/code-423n4/2023-04-rubicon/blob/main/contracts/RubiconMarket.sol#L538

## Tools Used

Manual review

## Recommended Mitigation Steps

Remove `this` keyword from both functions `RubiconMarket.batchOffer` and `RubiconMarket.batchRequote` so that the address of `msg.sender` is not the address of the `RubiconMarket` contract and be the address of the caller(user).
