# Solidity API

## Ladle

_Ladle orchestrates contract calls throughout the Yield Protocol v2 into useful and efficient user oriented features._

### constructor

```solidity
constructor(contract ICauldron cauldron, contract IWETH9 weth) public
```

### getVault

```solidity
function getVault(bytes12 vaultId_) internal view returns (bytes12 vaultId, struct DataTypes.Vault vault)
```

_Obtains a vault by vaultId from the Cauldron, and verifies that msg.sender is the owner
If bytes(0) is passed as the vaultId it tries to load a vault from the cache_

### getSeries

```solidity
function getSeries(bytes6 seriesId) internal view returns (struct DataTypes.Series series)
```

_Obtains a series by seriesId from the Cauldron, and verifies that it exists_

### getJoin

```solidity
function getJoin(bytes6 assetId) internal view returns (contract IJoin join)
```

_Obtains a join by assetId, and verifies that it exists_

### getPool

```solidity
function getPool(bytes6 seriesId) internal view returns (contract IPool pool)
```

_Obtains a pool by seriesId, and verifies that it exists_

### addIntegration

```solidity
function addIntegration(address integration, bool set) external
```

_Add or remove an integration._

### _addIntegration

```solidity
function _addIntegration(address integration, bool set) private
```

_Add or remove an integration._

### addToken

```solidity
function addToken(address token, bool set) external
```

_Add or remove a token that the Ladle can call `transfer` or `permit` on._

### _addToken

```solidity
function _addToken(address token, bool set) private
```

_Add or remove a token that the Ladle can call `transfer` or `permit` on._

### addJoin

```solidity
function addJoin(bytes6 assetId, contract IJoin join) external
```

_Add a new Join for an Asset, or replace an existing one for a new one.
There can be only one Join per Asset. Until a Join is added, no tokens of that Asset can be posted or withdrawn._

### addPool

```solidity
function addPool(bytes6 seriesId, contract IPool pool) external
```

_Add a new Pool for a Series, or replace an existing one for a new one.
There can be only one Pool per Series. Until a Pool is added, it is not possible to borrow Base._

### addModule

```solidity
function addModule(address module, bool set) external
```

Treat modules as you would Ladle upgrades. Modules have unrestricted access to the Ladle
storage, and can wreak havoc easily.
Modules must not do any changes to any vault (owner, seriesId, ilkId) because of vault caching.
Modules must not be contracts that can self-destruct because of `moduleCall`.
Modules can't use `msg.value` because of `batch`.

_Add or remove a module._

### setFee

```solidity
function setFee(uint256 fee) external
```

_Set the fee parameter_

### batch

```solidity
function batch(bytes[] calls) external payable returns (bytes[] results)
```

_Allows batched call to self (this contract)._

| Name | Type | Description |
| ---- | ---- | ----------- |
| calls | bytes[] | An array of inputs for each call. |

### route

```solidity
function route(address integration, bytes data) external payable returns (bytes result)
```

_Allow users to route calls to a contract, to be used with batch_

### moduleCall

```solidity
function moduleCall(address module, bytes data) external payable returns (bytes result)
```

_Allow users to use functionality coded in a module, to be used with batch_

### forwardPermit

```solidity
function forwardPermit(contract IERC2612 token, address spender, uint256 amount, uint256 deadline, uint8 v, bytes32 r, bytes32 s) external payable
```

_Execute an ERC2612 permit for the selected token_

### forwardDaiPermit

```solidity
function forwardDaiPermit(contract DaiAbstract token, address spender, uint256 nonce, uint256 deadline, bool allowed, uint8 v, bytes32 r, bytes32 s) external payable
```

_Execute a Dai-style permit for the selected token_

### transfer

```solidity
function transfer(contract IERC20 token, address receiver, uint128 wad) external payable
```

_Allow users to trigger a token transfer from themselves to a receiver through the ladle, to be used with batch_

### retrieve

```solidity
function retrieve(contract IERC20 token, address to) external payable returns (uint256 amount)
```

_Retrieve any token in the Ladle_

### receive

```solidity
receive() external payable
```

_The WETH9 contract will send ether to BorrowProxy on `weth.withdraw` using this function._

### joinEther

```solidity
function joinEther(bytes6 etherId) external payable returns (uint256 ethTransferred)
```

_Accept Ether, wrap it and forward it to the WethJoin
This function should be called first in a batch, and the Join should keep track of stored reserves
Passing the id for a join that doesn't link to a contract implemnting IWETH9 will fail_

### exitEther

```solidity
function exitEther(address payable to) external payable returns (uint256 ethTransferred)
```

_Unwrap Wrapped Ether held by this Ladle, and send the Ether
This function should be called last in a batch, and the Ladle should have no reason to keep an WETH balance_

### _generateVaultId

```solidity
function _generateVaultId(uint8 salt) private view returns (bytes12)
```

_Generate a vaultId. A keccak256 is cheaper than using a counter with a SSTORE, even accounting for eventual collision retries._

### build

```solidity
function build(bytes6 seriesId, bytes6 ilkId, uint8 salt) external payable virtual returns (bytes12, struct DataTypes.Vault)
```

_Create a new vault, linked to a series (and therefore underlying) and a collateral_

### _build

```solidity
function _build(bytes6 seriesId, bytes6 ilkId, uint8 salt) internal returns (bytes12 vaultId, struct DataTypes.Vault vault)
```

_Create a new vault, linked to a series (and therefore underlying) and a collateral_

### tweak

```solidity
function tweak(bytes12 vaultId_, bytes6 seriesId, bytes6 ilkId) external payable returns (struct DataTypes.Vault vault)
```

_Change a vault series or collateral._

### give

```solidity
function give(bytes12 vaultId_, address receiver) external payable returns (struct DataTypes.Vault vault)
```

_Give a vault to another user._

### destroy

```solidity
function destroy(bytes12 vaultId_) external payable
```

_Destroy an empty vault. Used to recover gas costs._

### stir

```solidity
function stir(bytes12 from, bytes12 to, uint128 ink, uint128 art) external payable
```

_Move collateral and debt between vaults._

### _pour

```solidity
function _pour(bytes12 vaultId, struct DataTypes.Vault vault, address to, int128 ink, int128 art) private
```

_Add collateral and borrow from vault, pull assets from and push borrowed asset to user
Or, repay to vault and remove collateral, pull borrowed asset from and push assets to user
Borrow only before maturity._

### pour

```solidity
function pour(bytes12 vaultId_, address to, int128 ink, int128 art) external payable
```

_Add collateral and borrow from vault, pull assets from and push borrowed asset to user
Or, repay to vault and remove collateral, pull borrowed asset from and push assets to user
Borrow only before maturity._

### serve

```solidity
function serve(bytes12 vaultId_, address to, uint128 ink, uint128 base, uint128 max) external payable returns (uint128 art)
```

_Add collateral and borrow from vault, so that a precise amount of base is obtained by the user.
The base is obtained by borrowing fyToken and buying base with it in a pool.
Only before maturity._

### close

```solidity
function close(bytes12 vaultId_, address to, int128 ink, int128 art) external payable returns (uint128 base)
```

_Repay vault debt using underlying token at a 1:1 exchange rate, without trading in a pool.
It can add or remove collateral at the same time.
The debt to repay is denominated in fyToken, even if the tokens pulled from the user are underlying.
The debt to repay must be entered as a negative number, as with `pour`.
Debt cannot be acquired with this function._

### repay

```solidity
function repay(bytes12 vaultId_, address to, int128 ink, uint128 min) external payable returns (uint128 art)
```

_Repay debt by selling base in a pool and using the resulting fyToken
The base tokens need to be already in the pool, unaccounted for.
Only before maturity. After maturity use close._

### repayVault

```solidity
function repayVault(bytes12 vaultId_, address to, int128 ink, uint128 max) external payable returns (uint128 base)
```

_Repay all debt in a vault by buying fyToken from a pool with base.
The base tokens need to be already in the pool, unaccounted for. The surplus base will be returned to msg.sender.
Only before maturity. After maturity use close._

### roll

```solidity
function roll(bytes12 vaultId_, bytes6 newSeriesId, uint8 loan, uint128 max) external payable returns (struct DataTypes.Vault vault, uint128 newDebt)
```

_Change series and debt of a vault._

### repayFromLadle

```solidity
function repayFromLadle(bytes12 vaultId_, address to) external payable returns (uint256 repaid)
```

_Use fyToken in the Ladle to repay debt. Return unused fyToken to `to`.
Return as much collateral as debt was repaid, as well. This function is only used when
removing liquidity added with "Borrow and Pool", so it's safe to assume the exchange rate
is 1:1. If used in other contexts, it might revert, which is fine._

### closeFromLadle

```solidity
function closeFromLadle(bytes12 vaultId_, address to) external payable returns (uint256 repaid)
```

_Use base in the Ladle to repay debt. Return unused base to `to`.
Return as much collateral as debt was repaid, as well. This function is only used when
removing liquidity added with "Borrow and Pool", so it's safe to assume the exchange rate
is 1:1. If used in other contexts, it might revert, which is fine._

### redeem

```solidity
function redeem(bytes6 seriesId, address to, uint256 wad) external payable returns (uint256)
```

_Allow users to redeem fyToken, to be used with batch.
If 0 is passed as the amount to redeem, it redeems the fyToken balance of the Ladle instead._

