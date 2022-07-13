# Solidity API

## NotionalMultiOracle

We value fCash assets at face value. We still need to account from conversions
between fCash's 8 decimals and the decimals of the underlying.

### SourceSet

```solidity
event SourceSet(bytes6 notionalId, bytes6 underlyingId, address underlying)
```

### Source

```solidity
struct Source {
  uint8 baseDecimals;
  uint8 quoteDecimals;
  bool set;
}
```

### FCASH_DECIMALS

```solidity
uint8 FCASH_DECIMALS
```

### sources

```solidity
mapping(bytes6 => mapping(bytes6 => struct NotionalMultiOracle.Source)) sources
```

### setSource

```solidity
function setSource(bytes6 notionalId, bytes6 underlyingId, contract IERC20Metadata underlying) external
```

_Set or reset an oracle source and its inverse_

### peek

```solidity
function peek(bytes32 baseId, bytes32 quoteId, uint256 amountBase) external view virtual returns (uint256 amountQuote, uint256 updateTime)
```

_Convert amountBase base into quote at the latest oracle price._

### get

```solidity
function get(bytes32 baseId, bytes32 quoteId, uint256 amountBase) external virtual returns (uint256 amountQuote, uint256 updateTime)
```

_Convert amountBase base into quote at the latest oracle price, updating state if necessary. Same as `peek` for this oracle._

### _peek

```solidity
function _peek(bytes6 baseId, bytes6 quoteId, uint256 amountBase) private view returns (uint256 amountQuote, uint256 updateTime)
```

_Convert amountBase base into quote at the latest oracle price._

