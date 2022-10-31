# Solidity API

## CompositeMultiOracle

### SourceSet

```solidity
event SourceSet(bytes6 baseId, bytes6 quoteId, contract IOracle source)
```

### PathSet

```solidity
event PathSet(bytes6 baseId, bytes6 quoteId, bytes6[] path)
```

### sources

```solidity
mapping(bytes6 => mapping(bytes6 => contract IOracle)) sources
```

### paths

```solidity
mapping(bytes6 => mapping(bytes6 => bytes6[])) paths
```

### setSource

```solidity
function setSource(bytes6 baseId, bytes6 quoteId, contract IOracle source) external
```

Set or reset a Yearn Vault Token oracle source and its inverse

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseId | bytes6 | id used for underlying base token |
| quoteId | bytes6 | id used for underlying quote token |
| source | contract IOracle | Oracle contract for source |

### setPath

```solidity
function setPath(bytes6 base, bytes6 quote, bytes6[] path) external
```

Set or reset an price path and its reverse path

| Name | Type | Description |
| ---- | ---- | ----------- |
| base | bytes6 | Id of base token |
| quote | bytes6 | Id of quote token |
| path | bytes6[] | Path from base to quote |

### peek

```solidity
function peek(bytes32 base, bytes32 quote, uint256 amountBase) external view virtual returns (uint256 amountQuote, uint256 updateTime)
```

Convert amountBase base into quote at the latest oracle price, through a path is exists.

| Name | Type | Description |
| ---- | ---- | ----------- |
| base | bytes32 | Id of base token |
| quote | bytes32 | Id of quote token |
| amountBase | uint256 | Amount of base to convert to quote |

| Name | Type | Description |
| ---- | ---- | ----------- |
| amountQuote | uint256 | Amount of quote token converted from base |
| updateTime | uint256 |  |

### get

```solidity
function get(bytes32 base, bytes32 quote, uint256 amountBase) external virtual returns (uint256 amountQuote, uint256 updateTime)
```

Convert amountBase base into quote at the latest oracle price, through a path is exists.

_This function is transactional_

| Name | Type | Description |
| ---- | ---- | ----------- |
| base | bytes32 | Id of base token |
| quote | bytes32 | Id of quote token |
| amountBase | uint256 | Amount of base to convert to quote |

| Name | Type | Description |
| ---- | ---- | ----------- |
| amountQuote | uint256 | Amount of quote token converted from base |
| updateTime | uint256 |  |

### _peek

```solidity
function _peek(bytes6 base, bytes6 quote, uint256 amountBase, uint256 updateTimeIn) private view returns (uint256 amountQuote, uint256 updateTimeOut)
```

Convert amountBase base into quote at the latest oracle price, through a path is exists.

| Name | Type | Description |
| ---- | ---- | ----------- |
| base | bytes6 | Id of base token |
| quote | bytes6 | Id of quote token |
| amountBase | uint256 | Amount of base to convert to quote |
| updateTimeIn | uint256 | Lowest updateTime value obtained received seen until now |

| Name | Type | Description |
| ---- | ---- | ----------- |
| amountQuote | uint256 | Amount of quote token converted from base |
| updateTimeOut | uint256 | Lower of current price's updateTime or updateTimeIn |

### _get

```solidity
function _get(bytes6 base, bytes6 quote, uint256 amountBase, uint256 updateTimeIn) private returns (uint256 amountQuote, uint256 updateTimeOut)
```

Convert amountBase base into quote at the latest oracle price, through a path is exists.

| Name | Type | Description |
| ---- | ---- | ----------- |
| base | bytes6 | Id of base token |
| quote | bytes6 | Id of quote token |
| amountBase | uint256 | Amount of base to convert to quote |
| updateTimeIn | uint256 | Lowest updateTime value obtained received seen until now |

| Name | Type | Description |
| ---- | ---- | ----------- |
| amountQuote | uint256 | Amount of quote token converted from base |
| updateTimeOut | uint256 | Lower of current price's updateTime or updateTimeIn |

