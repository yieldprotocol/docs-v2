# Solidity API

## StEthConverter

_A contract to handle wrapping & unwrapping of stETH_

### wstETH

```solidity
contract IWstETH wstETH
```

### stETH

```solidity
contract IERC20 stETH
```

### constructor

```solidity
constructor(contract IWstETH wstETH_, contract IERC20 stETH_) public
```

### wrap

```solidity
function wrap(address to) external returns (uint256 wstEthAmount)
```

_Wrap stEth held by this contract and forward it to the "to" address_

### unwrap

```solidity
function unwrap(address to) external returns (uint256 stEthAmount)
```

_Unwrap WstETH held by this contract, and send the stETH to the "to" address_

