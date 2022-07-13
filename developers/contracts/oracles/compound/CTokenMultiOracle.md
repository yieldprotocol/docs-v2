# Solidity API

## CTokenMultiOracle

### SourceSet

```solidity
event SourceSet(bytes6 baseId, bytes6 quoteId, contract CTokenInterface cToken)
```

### Source

```solidity
struct Source {
  contract CTokenInterface source;
  uint8 baseDecimals;
  uint8 quoteDecimals;
  bool inverse;
}
```

### sources

```solidity
mapping(bytes6 => mapping(bytes6 => struct CTokenMultiOracle.Source)) sources
```

### setSource

```solidity
function setSource(bytes6 cTokenId, bytes6 underlyingId, contract CTokenInterface cToken) external
```

_Set or reset an oracle source and its inverse_

### peek

```solidity
function peek(bytes32 base, bytes32 quote, uint256 amountBase) external view virtual returns (uint256 amountQuote, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price.

### get

```solidity
function get(bytes32 base, bytes32 quote, uint256 amountBase) external virtual returns (uint256 amountQuote, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price. Updates the price before fetching it if possible.

