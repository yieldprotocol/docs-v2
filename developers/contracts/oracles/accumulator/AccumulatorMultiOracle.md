# Solidity API

## AccumulatorMultiOracle

A collection of independent Accumulator Oracles

Each Accumulator is simple: it starts when `setSource` is called, 
and each `get` call returns perSecondRate ^ (time in seconds since oracle creation)

### Accumulator

```solidity
struct Accumulator {
  uint256 perSecondRate;
  uint256 accumulated;
  uint256 lastUpdated;
}
```

### sources

```solidity
mapping(bytes6 => mapping(bytes6 => struct AccumulatorMultiOracle.Accumulator)) sources
```

### SourceSet

```solidity
event SourceSet(bytes6 baseId, bytes6 kind, uint256 startRate, uint256 perSecondRate)
```

### PerSecondRateUpdated

```solidity
event PerSecondRateUpdated(bytes6 baseId, bytes6 kind, uint256 perSecondRate)
```

### setSource

```solidity
function setSource(bytes6 baseId, bytes6 kindId, uint256 startRate, uint256 perSecondRate) external
```

Set a source
    @param baseId: base to set the source for
    @param kindId: kind of oracle (example: chi/rate)
    @param startRate: rate the oracle starts with
    @param perSecondRate: secondly rate

### updatePerSecondRate

```solidity
function updatePerSecondRate(bytes6 baseId, bytes6 kindId, uint256 perSecondRate) external
```

Updates accumulation rate
    
    The accumulation rate can only be updated on an up-to-date oracle: get() was called in the
    same block. See get() for more details

### peek

```solidity
function peek(bytes32 base, bytes32 kind, uint256) external view virtual returns (uint256 accumulated, uint256 updateTime)
```

Retrieve the latest stored accumulated rate.

### get

```solidity
function get(bytes32 base, bytes32 kind, uint256) external virtual returns (uint256 accumulated, uint256 updateTime)
```

Retrieve the latest accumulated rate from source, updating it if necessary.

    Computes baseRate ^ (block.timestamp - creation timestamp)

    pow() is not O(1), so the naive implementation will become slower as the time passes
    To workaround that, each time get() is called, we:
        1) compute the return value
        2) store the return value in `accumulated` field, update lastUpdated timestamp

    Becase we have `accumulated`, step 1 becomes `accumulated * baseRate ^ (block.timestamp - lastUpdated)

