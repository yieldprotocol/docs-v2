
The Pool contract exchanges base for fyToken at a price defined by a specific formula.

## Functions
### constructor
```solidity
  function constructor(
  ) public
```




### setYield
```solidity
  function setYield(
  ) external
```
Use with extreme caution, only for Ladle replacements
Set a new Ladle and Cauldron



### setTokenId
```solidity
  function setTokenId(
  ) external
```
Use with extreme caution, only for token reconfigurations in Cauldron
Set a new base token id



### resetTokenJoin
```solidity
  function resetTokenJoin(
  ) external
```
Use with extreme caution, only for Join replacements
Reset the base token join



### startPool
```solidity
  function startPool(
    bytes6 seriesId_,
    uint256 minRatio,
    uint256 maxRatio
  ) external
```
When calling this function for the first pool, some underlying needs to be transferred to the strategy first, using a batchable router.
Start the strategy investments in the next pool

#### Parameters:
| Name | Type | Description                                                          |
| :--- | :--- | :------------------------------------------------------------------- |
|`seriesId_` | bytes6 | Yield v2 seriesId for the pool to invest in. 
|`minRatio` | uint256 | Minimum allowed ratio between the reserves of the next pool, as a fixed point number with 18 decimals (base/fyToken)
|`maxRatio` | uint256 | Maximum allowed ratio between the reserves of the next pool, as a fixed point number with 18 decimals (base/fyToken)


### endPool
```solidity
  function endPool(
  ) external
```

Divest out of a pool once it has matured


### mint
```solidity
  function mint(
  ) external returns (uint256 minted)
```
The lp tokens that the user contributes need to have been transferred previously, using a batchable router.
Mint strategy tokens.



### burn
```solidity
  function burn(
  ) external returns (uint256 withdrawal)
```
The strategy tokens that the user burns need to have been transferred previously, using a batchable router.
Burn strategy tokens to withdraw lp tokens. The lp tokens obtained won't be of the same pool that the investor deposited,
if the strategy has swapped to another pool.



### burnForBase
```solidity
  function burnForBase(
  ) external returns (uint256 withdrawal)
```
The strategy tokens that the user burns need to have been transferred previously, using a batchable router.
Burn strategy tokens to withdraw base tokens. It can be called only when a pool is not selected.



## Events
### YieldSet
```solidity
  event YieldSet(
  )
```



### TokenJoinReset
```solidity
  event TokenJoinReset(
  )
```



### TokenIdSet
```solidity
  event TokenIdSet(
  )
```



### PoolEnded
```solidity
  event PoolEnded(
  )
```



### PoolStarted
```solidity
  event PoolStarted(
  )
```



