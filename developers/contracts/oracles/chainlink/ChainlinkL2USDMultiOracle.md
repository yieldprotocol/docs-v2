# Solidity API

## ChainlinkL2USDMultiOracle

Chainlink recommends checking the sequencer status on some L2 networks to avoid
  reading stale data

  https://docs.chain.link/docs/l2-sequencer-flag/

### chainlinkFlags

```solidity
contract FlagsInterface chainlinkFlags
```

### FLAG_ARBITRUM_SEQ_OFFLINE

```solidity
address FLAG_ARBITRUM_SEQ_OFFLINE
```

### constructor

```solidity
constructor(contract FlagsInterface flags) public
```

### onlyFresh

```solidity
modifier onlyFresh()
```

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

