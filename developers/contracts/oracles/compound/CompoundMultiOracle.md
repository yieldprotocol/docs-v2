# Solidity API

## CompoundMultiOracle

### SourceSet

```solidity
event SourceSet(bytes6 baseId, bytes6 kind, address source)
```

### sources

```solidity
mapping(bytes6 => mapping(bytes6 => address)) sources
```

### setSource

```solidity
function setSource(bytes6 base, bytes6 kind, address source) external
```

Set or reset a source

### peek

```solidity
function peek(bytes32 base, bytes32 kind, uint256) external view virtual returns (uint256 accumulator, uint256 updateTime)
```

Retrieve the latest stored accumulator.

### get

```solidity
function get(bytes32 base, bytes32 kind, uint256) external virtual returns (uint256 accumulator, uint256 updateTime)
```

Retrieve the latest accumulator from source, updating it if necessary.

### _peek

```solidity
function _peek(bytes6 base, bytes6 kind) private view returns (uint256 accumulator, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price.

