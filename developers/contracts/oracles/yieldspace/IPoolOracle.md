# Solidity API

## IPoolOracle

### peek

```solidity
function peek(address pool) external view returns (uint256 twar)
```

returns the TWAR for a given `pool` using the moving average over the max available time range within the window

| Name | Type | Description |
| ---- | ---- | ----------- |
| pool | address | Address of pool for which the observation is required |

| Name | Type | Description |
| ---- | ---- | ----------- |
| twar | uint256 | The most up to date TWAR for `pool` |

### get

```solidity
function get(address pool) external returns (uint256 twar)
```

returns the TWAR for a given `pool` using the moving average over the max available time range within the window

_will try to record a new observation if necessary, so equivalent to `update(pool); peek(pool);`_

| Name | Type | Description |
| ---- | ---- | ----------- |
| pool | address | Address of pool for which the observation is required |

| Name | Type | Description |
| ---- | ---- | ----------- |
| twar | uint256 | The most up to date TWAR for `pool` |

### update

```solidity
function update(address pool) external
```

updates the cumulative ratio for the observation at the current timestamp. each observation is updated at most
once per epoch period.

| Name | Type | Description |
| ---- | ---- | ----------- |
| pool | address | Address of pool for which the observation should be recorded |

