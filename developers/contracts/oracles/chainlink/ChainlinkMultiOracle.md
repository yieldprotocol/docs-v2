# Solidity API

## ChainlinkMultiOracle

Chainlink only uses USD or ETH as a quote in the aggregators, and we will use only ETH

### SourceSet

```solidity
event SourceSet(bytes6 baseId, contract IERC20Metadata base, bytes6 quoteId, contract IERC20Metadata quote, address source)
```

### Source

```solidity
struct Source {
  address source;
  uint8 baseDecimals;
  uint8 quoteDecimals;
  bool inverse;
}
```

### sources

```solidity
mapping(bytes6 => mapping(bytes6 => struct ChainlinkMultiOracle.Source)) sources
```

### setSource

```solidity
function setSource(bytes6 baseId, contract IERC20Metadata base, bytes6 quoteId, contract IERC20Metadata quote, address source) external
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

### _peekThroughETH

```solidity
function _peekThroughETH(bytes6 baseId, bytes6 quoteId, uint256 amountBase) private view returns (uint256 amountQuote, uint256 updateTime)
```

_Convert amountBase base into quote at the latest oracle price, using ETH as an intermediate step._

