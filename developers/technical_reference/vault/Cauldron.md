


## Functions
### addAsset
```solidity
  function addAsset(
  ) external
```

Add a new Asset.


### setDebtLimits
```solidity
  function setDebtLimits(
  ) external
```

Set the maximum and minimum debt for an underlying and ilk pair. Can be reset.


### setLendingOracle
```solidity
  function setLendingOracle(
  ) external
```

Set a rate oracle. Can be reset.


### setSpotOracle
```solidity
  function setSpotOracle(
  ) external
```

Set a spot oracle and its collateralization ratio. Can be reset.


### addSeries
```solidity
  function addSeries(
  ) external
```

Add a new series


### addIlks
```solidity
  function addIlks(
  ) external
```

Add a new Ilk (approve an asset as collateral for a series).


### build
```solidity
  function build(
  ) external returns (struct DataTypes.Vault vault)
```

Create a new vault, linked to a series (and therefore underlying) and a collateral


### destroy
```solidity
  function destroy(
  ) external
```

Destroy an empty vault. Used to recover gas costs.


### _tweak
```solidity
  function _tweak(
  ) internal
```

Change a vault series and/or collateral types.


### tweak
```solidity
  function tweak(
  ) external returns (struct DataTypes.Vault vault)
```

Change a vault series and/or collateral types.
We can change the series if there is no debt, or assets if there are no assets


### _give
```solidity
  function _give(
  ) internal returns (struct DataTypes.Vault vault)
```

Transfer a vault to another user.


### give
```solidity
  function give(
  ) external returns (struct DataTypes.Vault vault)
```

Transfer a vault to another user.


### vaultData
```solidity
  function vaultData(
  ) internal returns (struct DataTypes.Vault vault_, struct DataTypes.Series series_, struct DataTypes.Balances balances_)
```




### debtFromBase
```solidity
  function debtFromBase(
  ) external returns (uint128 art)
```
Think about rounding up if using, since we are dividing.
Convert a debt amount for a series from base to fyToken terms.



### debtToBase
```solidity
  function debtToBase(
  ) external returns (uint128 base)
```

Convert a debt amount for a series from fyToken to base terms


### stir
```solidity
  function stir(
  ) external returns (struct DataTypes.Balances, struct DataTypes.Balances)
```

Move collateral and debt between vaults.


### _pour
```solidity
  function _pour(
  ) internal returns (struct DataTypes.Balances)
```

Add collateral and borrow from vault, pull assets from and push borrowed asset to user
Or, repay to vault and remove collateral, pull borrowed asset from and push assets to user


### pour
```solidity
  function pour(
  ) external returns (struct DataTypes.Balances)
```

Manipulate a vault, ensuring it is collateralized afterwards.
To be used by debt management contracts.


### slurp
```solidity
  function slurp(
  ) external returns (struct DataTypes.Balances)
```

Reduce debt and collateral from a vault, ignoring collateralization checks.
To be used by liquidation engines.


### roll
```solidity
  function roll(
  ) external returns (struct DataTypes.Vault, struct DataTypes.Balances)
```

Change series and debt of a vault.
The module calling this function also needs to buy underlying in the pool for the new series, and sell it in pool for the old series.


### level
```solidity
  function level(
  ) external returns (int256)
```

Return the collateralization level of a vault. It will be negative if undercollateralized.


### mature
```solidity
  function mature(
  ) external
```

Record the borrowing rate at maturity for a series


### _mature
```solidity
  function _mature(
  ) internal
```

Record the borrowing rate at maturity for a series


### accrual
```solidity
  function accrual(
  ) external returns (uint256)
```

Retrieve the rate accrual since maturity, maturing if necessary.


### _level
```solidity
  function _level(
  ) internal returns (int256)
```

Return the collateralization level of a vault. It will be negative if undercollateralized.


## Events
### AssetAdded
```solidity
  event AssetAdded(
  )
```



### SeriesAdded
```solidity
  event SeriesAdded(
  )
```



### IlkAdded
```solidity
  event IlkAdded(
  )
```



### SpotOracleAdded
```solidity
  event SpotOracleAdded(
  )
```



### RateOracleAdded
```solidity
  event RateOracleAdded(
  )
```



### DebtLimitsSet
```solidity
  event DebtLimitsSet(
  )
```



### VaultBuilt
```solidity
  event VaultBuilt(
  )
```



### VaultTweaked
```solidity
  event VaultTweaked(
  )
```



### VaultDestroyed
```solidity
  event VaultDestroyed(
  )
```



### VaultGiven
```solidity
  event VaultGiven(
  )
```



### VaultPoured
```solidity
  event VaultPoured(
  )
```



### VaultStirred
```solidity
  event VaultStirred(
  )
```



### VaultRolled
```solidity
  event VaultRolled(
  )
```



### SeriesMatured
```solidity
  event SeriesMatured(
  )
```



