# Solidity API

## ChainlinkUSDMultiOracle

Chainlink only uses USD or ETH as a quote in the aggregators, and we will use only USD

### SourceSet

```solidity
event SourceSet(bytes6 baseId, contract IERC20Metadata base, address source)
```

### Source

```solidity
struct Source {
  address source;
  uint8 baseDecimals;
}
```

### sources

```solidity
mapping(bytes6 => struct ChainlinkUSDMultiOracle.Source) sources
```

### setSource

```solidity
function setSource(bytes6 baseId, contract IERC20Metadata base, address source) external
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

### _getPriceInUSD

```solidity
function _getPriceInUSD(bytes6 baseId) private view returns (uint256 uintPrice, uint256 updateTime, uint8 baseDecimals)
```

_returns price for `baseId` in USD and base (not USD!) decimals_

### _peekThroughUSD

```solidity
function _peekThroughUSD(bytes6 baseId, bytes6 quoteId, uint256 amountBase) internal view returns (uint256 amountQuote, uint256 updateTime)
```

_Convert amountBase base into quote at the latest oracle price, using USD as an intermediate step._

