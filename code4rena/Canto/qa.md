# Canto

## QA Report

### L-01 `_safeMint()` should be used rather than `_mint()` wherever possible

_There are **1** instances of this issue:_

```solidity
File: /src/Turnstile.sol

92: _mint(_recipient, tokenId);
```

https://github.com/code-423n4/2022-11-canto/blob/main/CIP-001/src/Turnstile.sol

### L-02 `emit` function called early

_There are **3** instances of this issue:_

```solidity
File: /src/Turnstile.sol

95: emit Register(smartContract, _recipient, tokenId);

112: emit Assign(smartContract, _tokenId);

139: emit Withdraw(_tokenId, _recipient, _amount);
```

https://github.com/code-423n4/2022-11-canto/blob/main/CIP-001/src/Turnstile.sol

### N-01 Inconsistent spacing in comments

_There are **5** instances of this issue:_

```solidity
File: /src/Turnstile.sol

10: ///      If contract is using proxy pattern, it's possible to register retroactively, however past fees will be lost.

11: ///      Recipient withdraws fees by calling `withdraw(uint256,address,uint256)`.

82: ///         `msg.sender` is assumed to be a smart contract that earns fees. Only smart contract itself

83: ///         can register a fee receipient.

104: ///         Callable only by smart contract itself.
```

https://github.com/code-423n4/2022-11-canto/blob/main/CIP-001/src/Turnstile.sol
