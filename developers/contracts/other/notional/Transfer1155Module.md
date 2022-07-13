# Solidity API

## Transfer1155Module

_Module to allow the Ladle to transfer ERC1155 tokens_

### constructor

```solidity
constructor(contract ICauldron cauldron, contract IWETH9 weth) public
```

_We won't use these, but still we need them in the constructor to not be abstract._

### transfer1155

```solidity
function transfer1155(contract ERC1155 token, uint256 id, address receiver, uint128 wad, bytes data) external payable
```

_Allow users to trigger an ERC1155 token transfer from themselves to a receiver through the ladle, to be used with batch_

