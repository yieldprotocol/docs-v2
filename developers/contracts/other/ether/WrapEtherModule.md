# Solidity API

## WrapEtherModule

_Module to allow the Ladle to wrap Ether into WETH and transfer it to any destination_

### constructor

```solidity
constructor(contract ICauldron cauldron, contract IWETH9 weth) public
```

### wrap

```solidity
function wrap(address receiver, uint256 wad) external payable
```

_Allow users to wrap Ether in the Ladle and send it to any destination._

