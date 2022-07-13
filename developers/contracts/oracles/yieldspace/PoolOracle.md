# Solidity API

## PoolOracle

_This contract collects data from different YieldSpace pools to compute a TWAR using a SMA (https://www.investopedia.com/terms/s/sma.asp)
Adapted from https://github.com/Uniswap/v2-periphery/blob/master/contracts/examples/ExampleSlidingWindowOracle.sol_

### ObservationRecorded

```solidity
event ObservationRecorded(address pool, uint256 index, struct PoolOracle.Observation observation)
```

### NoObservationsForPool

```solidity
error NoObservationsForPool(address pool)
```

### MissingHistoricalObservation

```solidity
error MissingHistoricalObservation(address pool)
```

### InsufficientElapsedTime

```solidity
error InsufficientElapsedTime(address pool, uint256 elapsedTime)
```

### Observation

```solidity
struct Observation {
  uint256 timestamp;
  uint256 ratioCumulative;
}
```

### windowSize

```solidity
uint256 windowSize
```

### granularity

```solidity
uint256 granularity
```

### periodSize

```solidity
uint256 periodSize
```

### minTimeElapsed

```solidity
uint256 minTimeElapsed
```

### poolObservations

```solidity
mapping(address => struct PoolOracle.Observation[]) poolObservations
```

### constructor

```solidity
constructor(uint256 windowSize_, uint256 granularity_, uint256 minTimeElapsed_) public
```

### observationIndexOf

```solidity
function observationIndexOf(uint256 timestamp) public view returns (uint256 index)
```

_calculates the index of the observation corresponding to the given timestamp_

| Name | Type | Description |
| ---- | ---- | ----------- |
| timestamp | uint256 | The timestamp to calculate the index for |

| Name | Type | Description |
| ---- | ---- | ----------- |
| index | uint256 | The index corresponding to the `timestamp` |

### getOldestObservationInWindow

```solidity
function getOldestObservationInWindow(address pool) public view returns (struct PoolOracle.Observation o)
```

_returns the observation from the oldest epoch (at the beginning of the window) relative to the current time_

| Name | Type | Description |
| ---- | ---- | ----------- |
| pool | address | Address of pool for which the observation is required |

| Name | Type | Description |
| ---- | ---- | ----------- |
| o | struct PoolOracle.Observation | The oldest observation available for `pool` |

### update

```solidity
function update(address pool) public
```

updates the cumulative ratio for the observation at the current timestamp. each observation is updated at most
once per epoch period.

| Name | Type | Description |
| ---- | ---- | ----------- |
| pool | address | Address of pool for which the observation should be recorded |

### peek

```solidity
function peek(address pool) public view returns (uint256 twar)
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

### _getCurrentCumulativeRatio

```solidity
function _getCurrentCumulativeRatio(address pool) internal view returns (uint256 lastRatio)
```

