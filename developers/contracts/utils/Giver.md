# Solidity API

## Giver

### cauldron

```solidity
contract ICauldron cauldron
```

### bannedIlks

```solidity
mapping(bytes6 => bool) bannedIlks
```

### IlkBanned

```solidity
event IlkBanned(bytes6 ilkId)
```

Event emitted after an ilk is banned

| Name | Type | Description |
| ---- | ---- | ----------- |
| ilkId | bytes6 | Ilkid to be banned |

### constructor

```solidity
constructor(contract ICauldron cauldron_) public
```

### banIlk

```solidity
function banIlk(bytes6 ilkId, bool set) external
```

Function to ban

| Name | Type | Description |
| ---- | ---- | ----------- |
| ilkId | bytes6 | the ilkId to be banned |
| set | bool | bool value to ban/unban an ilk |

### give

```solidity
function give(bytes12 vaultId, address receiver) external returns (struct DataTypes.Vault vault)
```

A give function which allows the owner of vault to give the vault to another address

| Name | Type | Description |
| ---- | ---- | ----------- |
| vaultId | bytes12 | The vaultId of the vault to be given |
| receiver | address | The address to which the vault is being given to |

| Name | Type | Description |
| ---- | ---- | ----------- |
| vault | struct DataTypes.Vault | The vault which has been given |

### seize

```solidity
function seize(bytes12 vaultId, address receiver) external returns (struct DataTypes.Vault vault)
```

A give function which allows the authenticated address to give the vault of any user to another address

| Name | Type | Description |
| ---- | ---- | ----------- |
| vaultId | bytes12 | The vaultId of the vault to be given |
| receiver | address | The address to which the vault is being given to |

| Name | Type | Description |
| ---- | ---- | ----------- |
| vault | struct DataTypes.Vault | The vault which has been given |

