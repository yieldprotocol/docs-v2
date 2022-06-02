# LadleStorage

_Ladle orchestrates contract calls throughout the Yield Protocol v2 into useful and efficient user oriented features._

### JoinAdded

```solidity
event JoinAdded(bytes6 assetId, address join)
```

### PoolAdded

```solidity
event PoolAdded(bytes6 seriesId, address pool)
```

### ModuleAdded

```solidity
event ModuleAdded(address module, bool set)
```

### IntegrationAdded

```solidity
event IntegrationAdded(address integration, bool set)
```

### TokenAdded

```solidity
event TokenAdded(address token, bool set)
```

### FeeSet

```solidity
event FeeSet(uint256 fee)
```

### cauldron

```solidity
contract ICauldron cauldron
```

### router

```solidity
contract Router router
```

### weth

```solidity
contract IWETH9 weth
```

### borrowingFee

```solidity
uint256 borrowingFee
```

### cachedVaultId

```solidity
bytes12 cachedVaultId
```

### joins

```solidity
mapping(bytes6 => contract IJoin) joins
```

### pools

```solidity
mapping(bytes6 => contract IPool) pools
```

### modules

```solidity
mapping(address => bool) modules
```

### integrations

```solidity
mapping(address => bool) integrations
```

### tokens

```solidity
mapping(address => bool) tokens
```

### constructor

```solidity
constructor(contract ICauldron cauldron_, contract IWETH9 weth_) public
```

