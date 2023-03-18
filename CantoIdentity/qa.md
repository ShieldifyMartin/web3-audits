# Canto Identity

## QA Report

### L-01 Faulty check

Should use `||` instead of `&&`, because when one of these conditions is not men, caller should be considered unauthorized.

```solidity
178: if (
    cidNFTOwner != msg.sender &&
    getApproved[_cidNFTID] != msg.sender &&
    !isApprovedForAll[cidNFTOwner][msg.sender]
) revert NotAuthorizedForCIDNFT(msg.sender, _cidNFTID, cidNFTOwner);

250: if (
    cidNFTOwner != msg.sender &&
    getApproved[_cidNFTID] != msg.sender &&
    !isApprovedForAll[cidNFTOwner][msg.sender]
) revert NotAuthorizedForCIDNFT(msg.sender, _cidNFTID, cidNFTOwner);
```

https://github.com/code-423n4/2023-01-canto-identity/blob/main/src/CidNFT.sol

### N-01 Use scientific notation (e.g. `1e18`) rather than exponentiation (e.g. 10`**`18)

```solidity
17: uint256 public constant REGISTER_FEE = 100 * 10**18;
```

https://github.com/code-423n4/2023-01-canto-identity/blob/main/src/SubprotocolRegistry.sol
