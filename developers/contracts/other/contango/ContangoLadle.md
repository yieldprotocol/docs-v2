# Solidity API

## ContangoLadle

### constructor

```solidity
constructor(contract ICauldron cauldron, contract IWETH9 weth) public
```

### build

```solidity
function build(bytes6, bytes6, uint8) external payable returns (bytes12, struct DataTypes.Vault)
```

### deterministicBuild

```solidity
function deterministicBuild(bytes12 vaultId, bytes6 seriesId, bytes6 ilkId) external payable returns (struct DataTypes.Vault vault)
```

