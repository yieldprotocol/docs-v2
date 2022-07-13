# Solidity API

## ConvexModule

### constructor

```solidity
constructor(contract ICauldron cauldron_, contract IWETH9 weth_) public
```

### addVault

```solidity
function addVault(contract IConvexJoin convexJoin, bytes12 vaultId) external
```

Adds a vault to the user's vault list in the convex wrapper

| Name | Type | Description |
| ---- | ---- | ----------- |
| convexJoin | contract IConvexJoin | The address of the convex wrapper to which the vault will be added |
| vaultId | bytes12 | The vaultId to be added |

### removeVault

```solidity
function removeVault(contract IConvexJoin convexJoin, bytes12 vaultId, address account) external
```

Removes a vault from the user's vault list in the convex wrapper

| Name | Type | Description |
| ---- | ---- | ----------- |
| convexJoin | contract IConvexJoin | The address of the convex wrapper from which the vault will be removed |
| vaultId | bytes12 | The vaultId to be removed |
| account | address | The address of the user from whose list the vault is to be removed |

