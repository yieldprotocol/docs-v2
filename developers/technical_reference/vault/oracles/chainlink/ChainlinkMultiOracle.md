Chainlink only uses USD or ETH as a quote in the aggregators, and we will use only ETH


## Functions
### setSource
```solidity
  function setSource(
  ) external
```

Set or reset an oracle source and its inverse


### peek
```solidity
  function peek(
  ) external returns (uint256 amountQuote, uint256 updateTime)
```

Convert amountBase base into quote at the latest oracle price.


### get
```solidity
  function get(
  ) external returns (uint256 amountQuote, uint256 updateTime)
```

Convert amountBase base into quote at the latest oracle price, updating state if necessary. Same as `peek` for this oracle.


## Events
### SourceSet
```solidity
  event SourceSet(
  )
```



