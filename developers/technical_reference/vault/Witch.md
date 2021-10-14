


## Functions
### constructor
```solidity
  function constructor(
  ) public
```




### point
```solidity
  function point(
  ) external
```

Point to a different ladle


### setIlk
```solidity
  function setIlk(
  ) external
```

Governance function to set:
 - the auction duration to calculate liquidation prices
 - the proportion of the collateral that will be sold at auction start
 - the maximum collateral that can be auctioned at the same time
 - the minimum collateral that must be left when buying, unless buying all
 - The decimals for maximum and minimum


### auction
```solidity
  function auction(
  ) external
```

Put an undercollateralized vault up for liquidation.


### buy
```solidity
  function buy(
  ) external returns (uint256 ink)
```

Pay `base` of the debt in a vault in liquidation, getting at least `min` collateral.
Use `payAll` to pay all the debt, using `buy` for amounts close to the whole vault might revert.


### payAll
```solidity
  function payAll(
  ) external returns (uint256 ink)
```

Pay all debt from a vault in liquidation, getting at least `min` collateral.


## Events
### Point
```solidity
  event Point(
  )
```



### IlkSet
```solidity
  event IlkSet(
  )
```



### Bought
```solidity
  event Bought(
  )
```



### Auctioned
```solidity
  event Auctioned(
  )
```



