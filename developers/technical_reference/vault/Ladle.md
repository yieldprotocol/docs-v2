
Ladle orchestrates contract calls throughout the Yield Protocol v2 into useful and efficient user oriented features.

## Functions
### constructor
```solidity
  function constructor(
  ) public
```




### getVault
```solidity
  function getVault(
  ) internal returns (bytes12 vaultId, struct DataTypes.Vault vault)
```

Obtains a vault by vaultId from the Cauldron, and verifies that msg.sender is the owner
If bytes(0) is passed as the vaultId it tries to load a vault from the cache


### getSeries
```solidity
  function getSeries(
  ) internal returns (struct DataTypes.Series series)
```

Obtains a series by seriesId from the Cauldron, and verifies that it exists


### getJoin
```solidity
  function getJoin(
  ) internal returns (contract IJoin join)
```

Obtains a join by assetId, and verifies that it exists


### getPool
```solidity
  function getPool(
  ) internal returns (contract IPool pool)
```

Obtains a pool by seriesId, and verifies that it exists


### addIntegration
```solidity
  function addIntegration(
  ) external
```

Add or remove an integration.


### addToken
```solidity
  function addToken(
  ) external
```

Add or remove a token that the Ladle can call `transfer` or `permit` on.


### addJoin
```solidity
  function addJoin(
  ) external
```

Add a new Join for an Asset, or replace an existing one for a new one.
There can be only one Join per Asset. Until a Join is added, no tokens of that Asset can be posted or withdrawn.


### addPool
```solidity
  function addPool(
  ) external
```

Add a new Pool for a Series, or replace an existing one for a new one.
There can be only one Pool per Series. Until a Pool is added, it is not possible to borrow Base.


### addModule
```solidity
  function addModule(
  ) external
```
Treat modules as you would Ladle upgrades. Modules have unrestricted access to the Ladle
storage, and can wreak havoc easily.
Modules must not do any changes to any vault (owner, seriesId, ilkId) because of vault caching.
Modules must not be contracts that can self-destruct because of `moduleCall`.
Modules can't use `msg.value` because of `batch`.
Add or remove a module.



### setFee
```solidity
  function setFee(
  ) external
```

Set the fee parameter


### batch
```solidity
  function batch(
    bytes[] calls
  ) external returns (bytes[] results)
```

Allows batched call to self (this contract).

#### Parameters:
| Name | Type | Description                                                          |
| :--- | :--- | :------------------------------------------------------------------- |
|`calls` | bytes[] | An array of inputs for each call.

### route
```solidity
  function route(
  ) external returns (bytes result)
```

Allow users to route calls to a contract, to be used with batch


### moduleCall
```solidity
  function moduleCall(
  ) external returns (bytes result)
```

Allow users to use functionality coded in a module, to be used with batch


### forwardPermit
```solidity
  function forwardPermit(
  ) external
```

Execute an ERC2612 permit for the selected token


### forwardDaiPermit
```solidity
  function forwardDaiPermit(
  ) external
```

Execute a Dai-style permit for the selected token


### transfer
```solidity
  function transfer(
  ) external
```

Allow users to trigger a token transfer from themselves to a receiver through the ladle, to be used with batch


### retrieve
```solidity
  function retrieve(
  ) external returns (uint256 amount)
```

Retrieve any token in the Ladle


### receive
```solidity
  function receive(
  ) external
```

The WETH9 contract will send ether to BorrowProxy on `weth.withdraw` using this function.


### joinEther
```solidity
  function joinEther(
  ) external returns (uint256 ethTransferred)
```

Accept Ether, wrap it and forward it to the WethJoin
This function should be called first in a batch, and the Join should keep track of stored reserves
Passing the id for a join that doesn't link to a contract implemnting IWETH9 will fail


### exitEther
```solidity
  function exitEther(
  ) external returns (uint256 ethTransferred)
```

Unwrap Wrapped Ether held by this Ladle, and send the Ether
This function should be called last in a batch, and the Ladle should have no reason to keep an WETH balance


### build
```solidity
  function build(
  ) external returns (bytes12, struct DataTypes.Vault)
```

Create a new vault, linked to a series (and therefore underlying) and a collateral


### tweak
```solidity
  function tweak(
  ) external returns (struct DataTypes.Vault vault)
```

Change a vault series or collateral.


### give
```solidity
  function give(
  ) external returns (struct DataTypes.Vault vault)
```

Give a vault to another user.


### destroy
```solidity
  function destroy(
  ) external
```

Destroy an empty vault. Used to recover gas costs.


### stir
```solidity
  function stir(
  ) external
```

Move collateral and debt between vaults.


### pour
```solidity
  function pour(
  ) external
```

Add collateral and borrow from vault, pull assets from and push borrowed asset to user
Or, repay to vault and remove collateral, pull borrowed asset from and push assets to user
Borrow only before maturity.


### serve
```solidity
  function serve(
  ) external returns (uint128 art)
```

Add collateral and borrow from vault, so that a precise amount of base is obtained by the user.
The base is obtained by borrowing fyToken and buying base with it in a pool.
Only before maturity.


### close
```solidity
  function close(
  ) external returns (uint128 base)
```

Repay vault debt using underlying token at a 1:1 exchange rate, without trading in a pool.
It can add or remove collateral at the same time.
The debt to repay is denominated in fyToken, even if the tokens pulled from the user are underlying.
The debt to repay must be entered as a negative number, as with `pour`.
Debt cannot be acquired with this function.


### repay
```solidity
  function repay(
  ) external returns (uint128 art)
```

Repay debt by selling base in a pool and using the resulting fyToken
The base tokens need to be already in the pool, unaccounted for.
Only before maturity. After maturity use close.


### repayVault
```solidity
  function repayVault(
  ) external returns (uint128 base)
```

Repay all debt in a vault by buying fyToken from a pool with base.
The base tokens need to be already in the pool, unaccounted for. The surplus base will be returned to msg.sender.
Only before maturity. After maturity use close.


### roll
```solidity
  function roll(
  ) external returns (struct DataTypes.Vault vault, uint128 newDebt)
```

Change series and debt of a vault.


### repayFromLadle
```solidity
  function repayFromLadle(
  ) external returns (uint256 repaid)
```

Use fyToken in the Ladle to repay debt. Return unused fyToken to `to`.
Return as much collateral as debt was repaid, as well. This function is only used when
removing liquidity added with "Borrow and Pool", so it's safe to assume the exchange rate
is 1:1. If used in other contexts, it might revert, which is fine.


### closeFromLadle
```solidity
  function closeFromLadle(
  ) external returns (uint256 repaid)
```

Use base in the Ladle to repay debt. Return unused base to `to`.
Return as much collateral as debt was repaid, as well. This function is only used when
removing liquidity added with "Borrow and Pool", so it's safe to assume the exchange rate
is 1:1. If used in other contexts, it might revert, which is fine.


### redeem
```solidity
  function redeem(
  ) external returns (uint256)
```

Allow users to redeem fyToken, to be used with batch.
If 0 is passed as the amount to redeem, it redeems the fyToken balance of the Ladle instead.


