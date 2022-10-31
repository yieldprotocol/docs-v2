# Solidity API

## UniswapV3Oracle

### SourceSet

```solidity
event SourceSet(bytes6 base, bytes6 quote, address pool, uint32 twapInterval)
```

### Source

```solidity
struct Source {
  address pool;
  address baseToken;
  address quoteToken;
  uint32 twapInterval;
}
```

### sources

```solidity
mapping(bytes6 => mapping(bytes6 => struct UniswapV3Oracle.Source)) sources
```

### setSource

```solidity
function setSource(bytes6 base, bytes6 quote, address pool, uint32 twapInterval) external
```

Set or reset an oracle source, its inverse and twapInterval

### peek

```solidity
function peek(bytes32 base, bytes32 quote, uint256 amountBase) external view virtual returns (uint256 amountQuote, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price.

### get

```solidity
function get(bytes32 base, bytes32 quote, uint256 amountBase) external virtual returns (uint256 amountQuote, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price. Same as `peek` for this oracle.

### _peek

```solidity
function _peek(bytes6 base, bytes6 quote, uint256 amountBase) private view returns (uint256 amountQuote, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price.

### _setSource

```solidity
function _setSource(bytes6 base, bytes6 quote, address pool, uint32 twapInterval) internal
```

Set or reset an oracle source, its inverse and twapInterval

