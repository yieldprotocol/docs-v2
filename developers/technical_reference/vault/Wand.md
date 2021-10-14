
Ladle orchestrates contract calls throughout the Yield Protocol v2 into useful and efficient governance features.

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

Point to a different cauldron, ladle, witch, poolFactory, joinFactory or fyTokenFactory


### addAsset
```solidity
  function addAsset(
  ) external
```

Add an existing asset to the protocol, meaning:
 - Add the asset to the cauldron
 - Deploy a new Join, and integrate it with the Ladle
 - If the asset is a base, integrate its rate source
 - If the asset is a base, integrate a spot source and set a debt ceiling for any provided ilks


### makeBase
```solidity
  function makeBase(
  ) external
```
`oracle` must be able to deliver a value for assetId and 'rate'
Make a base asset out of a generic asset.



### makeIlk
```solidity
  function makeIlk(
  ) external
```
`oracle` must be able to deliver a value for baseId and ilkId
Make an ilk asset out of a generic asset.



### addSeries
```solidity
  function addSeries(
  ) external
```

Add an existing series to the protocol, by deploying a FYToken, and registering it in the cauldron with the approved ilks
This must be followed by a call to addPool


## Events
### Point
```solidity
  event Point(
  )
```



