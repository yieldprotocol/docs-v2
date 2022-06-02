# Solidity API

## CauldronMath

### add

```solidity
function add(uint128 x, int128 y) internal pure returns (uint128 z)
```

_Add a number (which might be negative) to a positive, and revert if the result is negative._

## Cauldron

### AssetAdded

```solidity
event AssetAdded(bytes6 assetId, address asset)
```

### SeriesAdded

```solidity
event SeriesAdded(bytes6 seriesId, bytes6 baseId, address fyToken)
```

### IlkAdded

```solidity
event IlkAdded(bytes6 seriesId, bytes6 ilkId)
```

### SpotOracleAdded

```solidity
event SpotOracleAdded(bytes6 baseId, bytes6 ilkId, address oracle, uint32 ratio)
```

### RateOracleAdded

```solidity
event RateOracleAdded(bytes6 baseId, address oracle)
```

### DebtLimitsSet

```solidity
event DebtLimitsSet(bytes6 baseId, bytes6 ilkId, uint96 max, uint24 min, uint8 dec)
```

### VaultBuilt

```solidity
event VaultBuilt(bytes12 vaultId, address owner, bytes6 seriesId, bytes6 ilkId)
```

### VaultTweaked

```solidity
event VaultTweaked(bytes12 vaultId, bytes6 seriesId, bytes6 ilkId)
```

### VaultDestroyed

```solidity
event VaultDestroyed(bytes12 vaultId)
```

### VaultGiven

```solidity
event VaultGiven(bytes12 vaultId, address receiver)
```

### VaultPoured

```solidity
event VaultPoured(bytes12 vaultId, bytes6 seriesId, bytes6 ilkId, int128 ink, int128 art)
```

### VaultStirred

```solidity
event VaultStirred(bytes12 from, bytes12 to, uint128 ink, uint128 art)
```

### VaultRolled

```solidity
event VaultRolled(bytes12 vaultId, bytes6 seriesId, uint128 art)
```

### SeriesMatured

```solidity
event SeriesMatured(bytes6 seriesId, uint256 rateAtMaturity)
```

### assets

```solidity
mapping(bytes6 => address) assets
```

### series

```solidity
mapping(bytes6 => struct DataTypes.Series) series
```

### ilks

```solidity
mapping(bytes6 => mapping(bytes6 => bool)) ilks
```

### lendingOracles

```solidity
mapping(bytes6 => contract IOracle) lendingOracles
```

### spotOracles

```solidity
mapping(bytes6 => mapping(bytes6 => struct DataTypes.SpotOracle)) spotOracles
```

### debt

```solidity
mapping(bytes6 => mapping(bytes6 => struct DataTypes.Debt)) debt
```

### ratesAtMaturity

```solidity
mapping(bytes6 => uint256) ratesAtMaturity
```

### vaults

```solidity
mapping(bytes12 => struct DataTypes.Vault) vaults
```

### balances

```solidity
mapping(bytes12 => struct DataTypes.Balances) balances
```

### addAsset

```solidity
function addAsset(bytes6 assetId, address asset) external
```

_Add a new Asset._

### setDebtLimits

```solidity
function setDebtLimits(bytes6 baseId, bytes6 ilkId, uint96 max, uint24 min, uint8 dec) external
```

_Set the maximum and minimum debt for an underlying and ilk pair. Can be reset._

### setLendingOracle

```solidity
function setLendingOracle(bytes6 baseId, contract IOracle oracle) external
```

_Set a rate oracle. Can be reset._

### setSpotOracle

```solidity
function setSpotOracle(bytes6 baseId, bytes6 ilkId, contract IOracle oracle, uint32 ratio) external
```

_Set a spot oracle and its collateralization ratio. Can be reset._

### addSeries

```solidity
function addSeries(bytes6 seriesId, bytes6 baseId, contract IFYToken fyToken) external
```

_Add a new series_

### addIlks

```solidity
function addIlks(bytes6 seriesId, bytes6[] ilkIds) external
```

_Add a new Ilk (approve an asset as collateral for a series)._

### build

```solidity
function build(address owner, bytes12 vaultId, bytes6 seriesId, bytes6 ilkId) external returns (struct DataTypes.Vault vault)
```

_Create a new vault, linked to a series (and therefore underlying) and a collateral_

### destroy

```solidity
function destroy(bytes12 vaultId) external
```

_Destroy an empty vault. Used to recover gas costs._

### _tweak

```solidity
function _tweak(bytes12 vaultId, bytes6 seriesId, bytes6 ilkId) internal returns (struct DataTypes.Vault vault)
```

_Change a vault series and/or collateral types.
We can change the series if there is no debt, or assets if there are no assets_

### tweak

```solidity
function tweak(bytes12 vaultId, bytes6 seriesId, bytes6 ilkId) external returns (struct DataTypes.Vault vault)
```

_Change a vault series and/or collateral types.
We can change the series if there is no debt, or assets if there are no assets_

### _give

```solidity
function _give(bytes12 vaultId, address receiver) internal returns (struct DataTypes.Vault vault)
```

_Transfer a vault to another user._

### give

```solidity
function give(bytes12 vaultId, address receiver) external returns (struct DataTypes.Vault vault)
```

_Transfer a vault to another user._

### vaultData

```solidity
function vaultData(bytes12 vaultId, bool getSeries) internal view returns (struct DataTypes.Vault vault_, struct DataTypes.Series series_, struct DataTypes.Balances balances_)
```

### debtFromBase

```solidity
function debtFromBase(bytes6 seriesId, uint128 base) external returns (uint128 art)
```

Think about rounding up if using, since we are dividing.

_Convert a debt amount for a series from base to fyToken terms._

### debtToBase

```solidity
function debtToBase(bytes6 seriesId, uint128 art) external returns (uint128 base)
```

_Convert a debt amount for a series from fyToken to base terms_

### stir

```solidity
function stir(bytes12 from, bytes12 to, uint128 ink, uint128 art) external returns (struct DataTypes.Balances, struct DataTypes.Balances)
```

_Move collateral and debt between vaults._

### _pour

```solidity
function _pour(bytes12 vaultId, struct DataTypes.Vault vault_, struct DataTypes.Balances balances_, struct DataTypes.Series series_, int128 ink, int128 art) internal returns (struct DataTypes.Balances)
```

_Add collateral and borrow from vault, pull assets from and push borrowed asset to user
Or, repay to vault and remove collateral, pull borrowed asset from and push assets to user_

### pour

```solidity
function pour(bytes12 vaultId, int128 ink, int128 art) external virtual returns (struct DataTypes.Balances)
```

_Manipulate a vault, ensuring it is collateralized afterwards.
To be used by debt management contracts._

### slurp

```solidity
function slurp(bytes12 vaultId, uint128 ink, uint128 art) external returns (struct DataTypes.Balances)
```

_Reduce debt and collateral from a vault, ignoring collateralization checks.
To be used by liquidation engines._

### roll

```solidity
function roll(bytes12 vaultId, bytes6 newSeriesId, int128 art) external returns (struct DataTypes.Vault, struct DataTypes.Balances)
```

_Change series and debt of a vault.
The module calling this function also needs to buy underlying in the pool for the new series, and sell it in pool for the old series._

### level

```solidity
function level(bytes12 vaultId) external returns (int256)
```

_Return the collateralization level of a vault. It will be negative if undercollateralized._

### mature

```solidity
function mature(bytes6 seriesId) external
```

_Record the borrowing rate at maturity for a series_

### _mature

```solidity
function _mature(bytes6 seriesId, struct DataTypes.Series series_) internal
```

_Record the borrowing rate at maturity for a series_

### accrual

```solidity
function accrual(bytes6 seriesId) external returns (uint256)
```

_Retrieve the rate accrual since maturity, maturing if necessary._

### _accrual

```solidity
function _accrual(bytes6 seriesId, struct DataTypes.Series series_) private returns (uint256 accrual_)
```

_Retrieve the rate accrual since maturity, maturing if necessary.
Note: Call only after checking we are past maturity_

### _level

```solidity
function _level(struct DataTypes.Vault vault_, struct DataTypes.Balances balances_, struct DataTypes.Series series_) internal returns (int256)
```

_Return the collateralization level of a vault. It will be negative if undercollateralized._

