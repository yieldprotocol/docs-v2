# Solidity API

## YieldSpaceMultiOracle

### SourceSet

```solidity
event SourceSet(bytes6 baseId, bytes6 quoteId, address pool, uint32 maturity, int128 ts, int128 g)
```

### Source

```solidity
struct Source {
  address pool;
  uint32 maturity;
  bool lending;
  int128 ts;
  int128 g;
}
```

### sources

```solidity
mapping(bytes6 => mapping(bytes6 => struct YieldSpaceMultiOracle.Source)) sources
```

### poolOracle

```solidity
contract IPoolOracle poolOracle
```

### wad64x64

```solidity
int128 wad64x64
```

### constructor

```solidity
constructor(contract IPoolOracle _poolOracle) public
```

### setSource

```solidity
function setSource(bytes6 seriesId, bytes6 baseId, address pool) external
```

Set or reset a FYToken oracle source and its inverse

_parameter ORDER IS crucial!  If id's are out of order the math will be wrong_

| Name | Type | Description |
| ---- | ---- | ----------- |
| seriesId | bytes6 | FYToken id |
| baseId | bytes6 | Underlying id |
| pool | address | Pool where you can trade FYToken <-> underlying |

### peek

```solidity
function peek(bytes32 base, bytes32 quote, uint256 amount) external view returns (uint256 value, uint256 updateTime)
```

Doesn't refresh the price, but returns the latest value available without doing any transactional operations:

| Name | Type | Description |
| ---- | ---- | ----------- |
| value | uint256 | in wei |
| updateTime | uint256 |  |

### get

```solidity
function get(bytes32 base, bytes32 quote, uint256 amount) external returns (uint256 value, uint256 updateTime)
```

Does whatever work or queries will yield the most up-to-date price, and returns it.

| Name | Type | Description |
| ---- | ---- | ----------- |
| value | uint256 | in wei |
| updateTime | uint256 |  |

### _source

```solidity
function _source(bytes32 base, bytes32 quote) internal view returns (struct YieldSpaceMultiOracle.Source source)
```

### _discount

```solidity
function _discount(struct YieldSpaceMultiOracle.Source source, uint256 amount, uint128 unitPrice, uint256 updateTime) internal view returns (uint256)
```

_Discount `amount` using the TWAR oracle rates. 
Lending => underlying to FYToken. Borrowing => FYToken to underlying_

| Name | Type | Description |
| ---- | ---- | ----------- |
| source | struct YieldSpaceMultiOracle.Source | Input params for the formulae |
| amount | uint256 | Amount to be discounted |
| unitPrice | uint128 | TWAR provided by the oracle |
| updateTime | uint256 | Time when the TWAR observation was calculated |

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | the discounted amount, <= `amount` when borrowing, >= `amount` when lending |

