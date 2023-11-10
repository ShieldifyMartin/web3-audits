# PartyDAO

## Report

### M-01 Possible DOS in `batchBurn` function because unbounded loop can run out of gas

In Crowdfund contract, the batchBurn() function might not be available to be called if there are a lot of contributors.

_There are **1** instances of this issue:_

```solidity
File: contracts/crowdfund/Crowdfund.sol

for (uint256 i = 0; i < contributors.length; ++i) {
    _burn(contributors[i], lc, party_);
}
```

https://github.com/PartyDAO/party-contracts-c4/blob/main/contracts/crowdfund/Crowdfund.sol
