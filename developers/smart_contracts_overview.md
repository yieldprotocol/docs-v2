# Smart Contracts Overview

## Utility Contracts

### AccessControl

The access control contract was adapted from OpenZeppelin's AccessControl.sol and is inherited from most other contracts in the Yield Protocol.

A role exists implicitly for each function in a contract, with the ROOT role as the admin for the role.

If the `auth` modifier is present in a function, access must have been granted to the caller by an account with the admin role for the function role. This admin role will usually be ROOT, but that can be changed.

An `admin` modifier exists to restrict functions to accounts bearing the `admin` role of a given other role. This is not used outside AccessControl.sol.

An account belonging to the admin role for a function can grant and revoke memberships to the function role.

The ROOT role is special in that it is its own admin so that any member of ROOT can grant and revoke ROOT permissions on other accounts.

There is a special LOCK role, that is also an admin of itself, but has no members. By changing the admin role of a function role to LOCK, no further changes can ever be done to the function role membership, except users voluntarily renouncing to the function role.

### ERC20 and ERC20Permit

The Yield Protocol uses its own implementation of ERC20 contracts, which borrow heavily from the DS-Token implementation. The ERC2162 extension is taken from WETH10, which was taken in turn from Yield v1.

## Vault Contracts

### Oracle

The Yield Protocol uses its own Oracle interface as a wrapper to external oracles, so that it is possible to switch to external oracles with arbitrary interfaces with no impact on the core contracts.

The IOracle interface offers a `get(bytes32 base, bytes32 quote, uint256 amount) returns (uint256 value)` function that accepts base and quote identifiers and an amount of `base` to convert according to them and to the value returned by the external oracle.

`peek` is the same as `get`, but for oracles that offer non-transactional readings.

### Spot oracles

Spot oracles return the `value` of an `amount` of `base` in `quote` terms. We use these to determine the collateralization level of vaults.

### Rate and chi oracles

To charge borrowers for maintaining a borrowing position after maturity, Yield charges interest. The interest is determined by an oracle called the ‘rate’ oracle. Likewise, to compensate lenders who leave fyTokens unredeemed, fyTokens increase in redemption value after maturity based upon an oracle called the ‘chi’ oracle.

Rate and chi oracles return the `value` of an `amount` multiplied by the borrowing and lending rates for `base`, respectively. These rates we currently get from Compound cTokens. We use these oracles to determine the debt of vaults after maturity, and the redemption value of fyToken after maturity.

### Multioracles

Multioracles store multiple sources of the same type in a single contract, to reduce deployment costs.

## Join Contracts

### Join

Join contracts hold ERC20 and other assets that are external to the Yield Protocol, but that we manage as collateral or underlying.

We deploy one Join for each asset (collateral or underlying) that is accepted in the Yield Protocol. The Ladle keeps a registry of these Joins and is the only contract or account with permissions to move assets in or out of a Join.

The Join contract keeps track of the assets it should be holding, instead of relying on checking its balance at the asset contract. This allows removing the need for approvals in the integration with other contracts.

<!-- TODO: Expand upon and add more clarity -->

***Join***

The `join` function takes assets from the specified account. If there are any unaccounted assets already in the Join, these are used before transferring from the user.

***Exit***

Transfer an amount of asset to the given address.

***Retrieve***

Transfer an amount of any ERC20 other than asset to the given address,

to enable airdrops to the Join.

<!-- TODO: Expand upon and add more clarity -->

***Flash Loans***

The Join can serve as a lender for ERC3156 compliant flash loans of the asset it holds.

***Join Factory***

To reduce the deployment size of Wand to under 24Kb, Joins are deployed through a Join Factory.

### FyToken

fyTokens are Ethereum based ERC-20 tokens that can be redeemed for an underlying asset one-to-one after a predetermined maturity date.

Redemptions can be executed at or after maturity. In a redemption, the fyTokens will be burnt and the underlying asset will be sent to a user on a one-to-one basis.

The fyTokens for redemption needs to either have been approved by the caller or needs to have been transferred to the fyToken contract. If transferring, the `transfer` and the `redeem` should happen in the same transaction.

On the first redemption or the first time that `mature` is called, the chi oracle is read to record the chi value at maturity. On any subsequent redemptions, the chi oracle is read to calculate the chi accrual, defined as `current chi / chi at maturity`.

The amount of underlying transferred on redemption is the amount of fyToken redeemed multiplied by the chi accrual.

On redemption, fyToken instructs the Join for the underlying to transfer the underlying to the target account.

The `mint` and `burn` functions are restricted, and only the Ladle can call them. It does so when issuing or repaying debt.

Can be flash minted with no fees following the ERC3156 standard.

<!-- TODO: Expand upon -->

For a given underlying asset, such as Dai, there would be a fyToken contract for each maturity. If, for example, we decide to have quarterly maturities for Dai in 2021, we would deploy 4 fyDai contracts: 31/03/21, 30/06/21, 30/09/21, and 31/12/21.

## Cauldron

The Cauldron is the debt and collateral accounting system for Yield. The only external dependencies from Cauldron are towards rate oracles and spot oracles. All transactional functions in Cauldron require privileged access.

### Vaults

User debt and collateral are recorded in numbered vaults so that each account can have multiple independent positions. A series of fyToken is a specific ERC-20 contract that has a maturity time and an underlying or “base” asset. Vaults permit an owner to provide a single kind of collateral to borrow a single series ****of fyTokens. Vaults can be created, modified, transferred, or destroyed.

Each vault is linked to a single series and a single ilk. A series is a fyToken contract with associated data. An ilk is an asset that is used as collateral.

The vault data is divided into two separate structs, both accessible through the vaultId: Vault and Balances.

DataTypes.Vault records the owner, seriesId and ilkId.

DataTypes.Balances record the vault debt(art) and vault collateral(ink). You use ink to draw art.

<!-- TODO: Ask Alberto why two structs for vaults - Sanket -->

### Stir and pour

`stir` moves collateral and debt between vaults. It can only move debt or collateral between vaults with matching seriesId or ilkId, respectively.

It can be combined with `build` and `destroy` to split and merge vaults.

`pour` is used to add and remove debt and collateral to vaults. It underpins borrowing and repaying.

`stir` and `pour` will revert if any affected vaults would be undercollateralized at the end of the transaction.

<!-- TODO: Ask Alberto why do we need both. It seems like they can be combined or one fulfills the needs. -->

### Debt after maturity

The debt of a vault is stored in fyToken terms, and only ever changes through user action.

However, if the debt is expressed in underlying assets, it increases after maturity through applying a borrowing rate obtained from the rate oracle for the underlying.

A different way to look at this mechanism

- 1 fyDai of debt can always be repaid with 1 fyDai
- 1 fyDai of debt can be paid with 1 Dai before maturity, or 1 Dai * rate_accrual after maturity, where `rate_accrual = borrowing_rate_now / borrowing_rate_at_maturity`

<!-- TODO: Not sure about the placement of this here - Sanket -->

## Collateralization

All vaults need to remain collateralized, or they will be at risk of liquidation.

A vault is collateralized if the value of its collateral is greater than the value of its debt by a factor equal to the collateralization ratio of the underlying and collateral pair.

`ink * price * ratio >= art * rate_accrual`

## Liquidations

For the protocol to not go bankrupt, it must act when the value of the collateral in the vaults risks being lower than the debt owed by the vaults.

Liquidation engines are given access to the Cauldron to resolve the situation. Any undercollateralized vault can be seized by a liquidation engine using `grab`. At that point, the vault effectively belongs to the liquidation engine.

For a time interval, defined as `auctionInterval`, the liquidations engine can run its algorithm to sell the collateral in the vault for underlying. If the collateralization ratio was over 100%, this sale can happen at a profit to the liquidation engine.

At the end of the collateral auction, the liquidation engine will return any obtained underlying to the appropriate Join and instruct the Cauldron to return the vault to its owner, with the debt reset to zero, and an amount of collateral that depends on the outcome of the auction.

Liquidation engines can have multiple implementations, but they must be approved by the Cauldron before they can operate.

### Protocol Debt

The Cauldron also records the protocol debt (DataTypes.Debt.debt) for every underlying and collateral pair.

As users borrow from a given series, the debt is accounted in the protocol totals as the underlying from the series and the collateral that was used.

There are ceiling debt limits (DataTypes.Debt.max), and the Cauldron will reject registering any more debt that surpasses them.

## Witch

The Witch is a Liquidation Engine, one of many possible implementations.

The Witch grabs uncollateralized vaults, replacing the owner by itself. Then it sells the vault collateral in exchange for underlying to pay its debt. The amount of collateral given increases over time, until it offers to sell all the collateral for underlying to pay all the debt. The auction is held open at the final price indefinitely.

After the debt is settled, the Witch returns the vault to its original owner and calls the Ladle to move assets, and the Cauldron to obtain and release control of undercollateralized vaults.

## Ladle

The Ladle is a routing and asset management contract for Yield. It can be upgraded through Modules or replaced entirely. It has considerable privileges and is the largest contract in the protocol.

***Cauldron***

The Ladle is the only contract that is authorized to make changes to the accounting in Cauldron. It is as well the only contract that is authorized to create, modify or destroy Vaults in the Cauldron.

***Joins***

The Ladle keeps a registry of all Joins and it is authorized to move assets from any Join to any account. The Ladle also moves assets from users to Joins, with allowances approved by the users.

***FyToken***

The Ladle is authorized to mint fyToken at will. The Ladle also moves fyToken from users to FYToken contracts for burning, with allowances approved by the users. The Ladle knows about all the existing fyTokens through the series registry in the Cauldron.

***Pools***

The Ladle keeps a registry of all the Pools, indexed by the id of the series traded. The Ladle also moves assets from users to Pool contracts for trading, with allowances approved by the users.

***Pour***

The Ladle uses the integrations and authorizations described above to provide collateralized debt features.

When a user wants to deposit collateral and borrow a fyToken, the Ladle will `pour`:

1. Take the collateral from the user to the appropriate Join.
2. Register the position in the Cauldron.
3. Mint the fyToken in the user’s account.

Similarly, a user can repay debt and withdraw collateral with `pour`.

1. Take the fyToken from the user, and burn it.
2. Register the new position in the Cauldron.
3. Transfer the collateral from the Join to the user.

***Close***

Users can also pay their debt with the underlying asset for the debt, instead of with fyTokens. While the debt in fyToken terms only changes through borrowing actions, the debt in underlying terms changes according to the matching rate oracle. The Ladle will get the rate accrual from the oracle through the Cauldron, and adjust asset transfers, and record positions accordingly.

***Stir***

Users can move collateral and debt between Vaults through the Ladle using `stir`. Combined with the creation and destruction of Vaults, this can be used to split a Vault in two or to merge two Vaults into one.

***Other Features***

- *Serve*: Borrow and trade the fyToken for underlying, effectively borrowing at a fixed rate.
- *Roll*: Migrate debt between two series.
- *Repay*: Trade underlying for fyToken, to pay the debt.
- *Repay Vault*: Trade underlying for fyToken, to pay exactly all the debt in a vault.
- *Remove and Repay*: Remove liquidity from a YieldSpace pool, and use the obtained fyToken to repay debt, if it exists.
- *Transfer to Pool and Route*: Move assets from the user to a pool, and execute a function in the latter. Can be used to sell underlying, effectively lending at a fixed rate.
- *Transfer to fyToken and Redeem*: Move assets from the user to a fyToken, and redeem them for underlying. This can be used to roll a mature lending position.
- *Permit management*: **Execute a Dai or ERC2162 off-chain permit.
- *Ether management*: Convert to WETH, or from WETH.

***Settle***

Settle is a restricted convenience function to allow the Witch to use the Joins, without setting individual permissions. It is used when liquidators buy auctioned collateral with underlying, to move the assets.

***Batch***

Any number of actions can be batched in a single transaction using the Ladle. In reality, batches are the only entry points to the Ladle.

Instead of using the popular “multicall” pattern, we have implemented a “switch” pattern that avoids external calls when batching, and allows us to cache the Vault that is being operated upon between actions.

***Modules***

The Ladle can be extended by the use of modules. The Ladle can `delegatecall` functions in modules that have been authorized via governance. The Modules can inherit from LadleStorage to read and modify the Ladle storage, although modifying it is discouraged.

***Wand***

Bundling of discrete governance actions into useful governance features.

- Add asset
- Make base
- Make ilk
- Add series