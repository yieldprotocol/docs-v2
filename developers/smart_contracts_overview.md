# Smart Contracts Overview


<figure class="image" align = "center">
  <img src="assets/architecture.png" width="1200" alt="Yield v2 smart contract architecture" title="Yield v2 smart contract architecture">
</figure>



# Vault Contracts

The vault-v2 repository contains the contracts that enable Yield v2 as a Collateralized Debt Platform. Together, these contracts allow users to post collateral and borrow fyToken, while the protocol takes care of topics such as accounting, collateralization, redemption and liquidations.


## Oracle

The Yield Protocol uses its own Oracle interface as a wrapper to external oracles, so that it is possible to switch to external oracles with arbitrary interfaces with no impact on the core contracts.

The IOracle interface offers a `get(bytes32 base, bytes32 quote, uint256 amount) returns (uint256 value)` function that accepts base and quote identifiers and an amount of `base` to convert according to them and to the value returned by the external oracle.

`peek` is the same as `get`, but for oracles that offer non-transactional readings.

Each oracle stores multiple sources of the same type in a single contract, to reduce deployment costs.


### Spot oracles

Spot oracles return the `value` of an `amount` of `base` in `quote` terms. We use these to determine the collateralization level of vaults.


### Rate and chi oracles

To charge borrowers for maintaining a borrowing position after maturity, Yield charges interest. The interest is determined by an oracle called the ‘rate’ oracle. Likewise, to compensate lenders who leave fyTokens unredeemed, fyTokens increase in redemption value after maturity based upon an oracle called the ‘chi’ oracle.

Rate and chi oracles return the `value` of an `amount` multiplied by the borrowing and lending rates for `base`, respectively. These rates we currently get from Compound cTokens. We use these oracles to determine the debt of vaults after maturity, and the redemption value of fyToken after maturity.


## Join

Join contracts hold ERC20 and other assets that are external to the Yield Protocol, but that we manage as collateral or underlying.

We deploy one Join for each asset (collateral or underlying) that is accepted in the Yield Protocol. The Ladle keeps a registry of these Joins and is the only contract or account with permissions to move assets in or out of a Join.

The Join contract keeps track of the assets it should be holding, instead of relying on checking its balance at the asset contract. This allows removing the need for approvals in the integration with other contracts.


### Join

The `join` function takes assets from the specified account. If there are any unaccounted assets already in the Join, these are used before transferring from the user.


### Exit

Transfer an amount of asset to the given address.


### Retrieve

Transfer an amount of any ERC20 other than asset to the given address, to enable airdrops to the Join.

Flash Loans

The Join can serve as a lender for ERC3156 compliant flash loans of the asset it holds.

Join Factory

To reduce the deployment size of Wand to under 24Kb, Joins are deployed through a Join Factory.


## FyToken

fyTokens are Ethereum based ERC-20 tokens that can be redeemed for an underlying asset one-to-one after a predetermined maturity date.

Redemptions can be executed at or after maturity. In a redemption, the fyTokens will be burnt and the underlying asset will be sent to a user on a one-to-one basis.

The fyTokens for redemption needs to either have been approved by the caller or needs to have been transferred to the fyToken contract. If transferring, the `transfer` and the `redeem` should happen in the same transaction.

On the first redemption or the first time that `mature` is called, the chi oracle is read to record the chi value at maturity. On any subsequent redemptions, the chi oracle is read to calculate the chi accrual, defined as `current chi / chi at maturity`.

The amount of underlying transferred on redemption is the amount of fyToken redeemed multiplied by the chi accrual.

On redemption, fyToken instructs the Join for the underlying to transfer the underlying to the target account.

The `mint` and `burn` functions are restricted, and only the Ladle can call them. It does so when issuing or repaying debt.

Can be flash minted with no fees following the ERC3156 standard.

For a given underlying asset, such as Dai, there would be a fyToken contract for each maturity. If, for example, we decide to have quarterly maturities for Dai in 2021, we would deploy 4 fyDai contracts: 31/03/21, 30/06/21, 30/09/21, and 31/12/21.


## Cauldron

The Cauldron is the debt and collateral accounting system for Yield. The only external dependencies from Cauldron are towards rate oracles and spot oracles. All transactional functions in Cauldron require privileged access.


### Vaults

User debt and collateral are recorded in numbered vaults so that each account can have multiple independent positions. A series of fyToken is a specific ERC-20 contract that has a maturity time and an underlying or “base” asset. Vaults permit an owner to provide a single kind of collateral to borrow a single series of fyTokens. Vaults can be created, modified, transferred, or destroyed.

Each vault is linked to a single series and a single ilk. A series is a fyToken contract with associated data. An ilk is an asset that is used as collateral.

The vault data is divided into two separate structs, both accessible through the vaultId: Vault and Balances.

DataTypes.Vault records the owner, seriesId and ilkId.

DataTypes.Balances record the vault debt(art) and vault collateral(ink). You use ink to draw art.


### Stir and pour

`stir` moves collateral and debt between vaults. It can only move debt or collateral between vaults with matching seriesId or ilkId, respectively.

It can be combined with `build` and `destroy` to split and merge vaults.

`pour` is used to add and remove debt and collateral to vaults. It underpins borrowing and repaying.

`stir` and `pour` will revert if any affected vaults would be undercollateralized at the end of the transaction.


### Debt after maturity

The debt of a vault is stored in fyToken terms, and only ever changes through user action.

However, if the debt is expressed in underlying assets, it increases after maturity through applying a borrowing rate obtained from the rate oracle for the underlying.

A different way to look at this mechanism



* 
1 fyDai of debt can always be repaid with 1 fyDai


* 
1 fyDai of debt can be paid with 1 Dai before maturity, or 1 Dai * rate_accrual after maturity, where `rate_accrual = borrowing_rate_now / borrowing_rate_at_maturity`

### Collateralization

All vaults need to remain collateralized, or they will be at risk of liquidation.

A vault is collateralized if the value of its collateral is greater than the value of its debt by a factor equal to the collateralization ratio of the underlying and collateral pair.


```
ink * price * ratio >= art * rate_accrual
```



### Liquidations

For the protocol to not go bankrupt, it must act when the value of the collateral in the vaults risks being lower than the debt owed by the vaults.

Liquidation engines are given access to the Cauldron to resolve the situation. Any undercollateralized vault can be seized by a liquidation engine using `grab`. At that point, the vault effectively belongs to the liquidation engine.

As the auction proceeds, the liquidation engine will use `settle` to send any obtained underlying to the appropriate Join, and the sold collateral to the auction buyers.

Once all the debt is sold, the liquidation engine will return the vault to its owner, with the debt reset to zero, and an amount of collateral that depends on the outcome of the auction.

Liquidation engines can have multiple implementations, but they must be approved by the Cauldron before they can operate.


### Protocol Debt

The Cauldron also records the protocol debt (DataTypes.Debt.debt) for every underlying and collateral pair.

As users borrow from a given series, the debt is accounted in the protocol totals as the underlying from the series and the collateral that was used.

There are ceiling debt limits (DataTypes.Debt.max), and the Cauldron will reject registering any more debt that surpasses them.


## Witch

The Witch is a Liquidation Engine, one of many possible implementations.

The Witch grabs uncollateralized vaults, replacing the owner by itself. Then it sells the vault collateral in exchange for underlying to pay its debt. The amount of collateral given increases over time, until it offers to sell all the collateral for underlying to pay all the debt. The auction is held open at the final price indefinitely.

After the debt is settled, the Witch returns the vault to its original owner.


## Ladle

The Ladle is a routing and asset management contract for Yield. It can be upgraded through Modules or replaced entirely. It has considerable privileges and is the most complex contract in the protocol.

The Ladle is authorized to make changes to the accounting in Cauldron. It is as well the only contract that is authorized to create, modify or destroy Vaults in the Cauldron.

The Ladle keeps a registry of all Joins and it is authorized to move assets from any Join to any account. The Ladle also moves assets from users to Joins, with allowances approved by the users.

The Ladle is authorized to mint fyToken at will. The Ladle also moves fyToken from users to FYToken contracts for burning, with allowances approved by the users. The Ladle knows about all the existing fyTokens through the series registry in the Cauldron.

The Ladle keeps a registry of all the Pools, indexed by the id of the series traded. The Ladle also moves assets from users to Pool contracts for trading, with allowances approved by the users.


### Execution Flow

Any number of actions can be batched in a single transaction using the Ladle using `batch`. This is the most common method of operation.

The Ladle can also be used to execute arbitrary calls on any registered contracts using `route`. This is used, for example, to deal with Pools and Strategies.

The Ladle can be extended by the use of modules. The Ladle can `moduleCall` functions in modules that have been authorized via governance. The Modules can inherit from LadleStorage to read and modify the Ladle storage, although modifying it is discouraged.

### User Features

The Ladle uses the integrations and authorizations described above to provide collateralized debt features.

When a user wants to deposit collateral and borrow a fyToken, the Ladle will `pour`:

1. Take the collateral from the user to the appropriate Join.
2. Register the position in the Cauldron.
3. Mint the fyToken in the user’s account.

Similarly, a user can repay debt and withdraw collateral with `pour`.
1. Take the fyToken from the user, and burn it.
2. Register the new position in the Cauldron.
3. Transfer the collateral from the Join to the user.

Users can also pay their debt with the underlying asset for the debt, instead of with fyTokens, using `close`. While the debt in fyToken terms only changes through borrowing actions, the debt in underlying terms changes according to the matching rate oracle. The Ladle will get the rate accrual from the oracle through the Cauldron, and adjust asset transfers, and record positions accordingly.

Users can move collateral and debt between Vaults through the Ladle using `stir`. Combined with the creation and destruction of Vaults, this can be used to split a Vault in two or to merge two Vaults into one.

_Other Features_
* _Serve_: Borrow and trade the fyToken for underlying, effectively borrowing at a fixed rate.
* _Roll_: Migrate debt between two series.
* _Repay_: Trade underlying for fyToken, to pay the debt.
* _Repay Vault_: Trade underlying for fyToken, to pay exactly all the debt in a vault.
* _RepayFromLadle_: Use any existing fyToken in the Ladle to repay debt.
* _CloseFromLadle_: Use any existing base in the Ladle to repay debt.
* _Redeem_: Redeem fyToken in a fyToken contract for underlying.
* _Transfer_: Take tokens from the caller to a destination.
* _Permit management_: Execute a Dai or ERC2162 off-chain permit.
* _Ether management_: Convert to WETH, or from WETH.

## Wand

Bundling of discrete governance actions into useful governance features.

*Add asset*: Adding an asset involves registering it in the Cauldron, deploying a Join for it, registering the Join in the Ladle, and permissioning the Ladle to use the Join.
*Make base*: Making a base out of an existing asset involves setting an oracle for its borrowing rate in the Cauldron, and allowing the Witch to put base into its join during liquidations.
*Make ilk*: Making an ilk out of an existing asset, for a given base, involves setting a spot oracle for the base/ilk pair in the Cauldron, setting debt limits in the Cauldron, and allowing the Witch to take such ilk out of its Join during liquidations.
*Add series*: Adding a series is a costly process in terms of gas. It involves deploying the related fyToken, registering it in the Cauldron, and approving a number of ilks to be used as collateral when borrowing it. The fyToken also need to be given access to the Join for its underlying. The Ladle must be given permission to mint and burn the added fyToken. A Pool to trade between the fyToken and its underlying must be deployed, and registered in the Ladle.

# YieldSpace

YieldSpace is an automated liquidity provider that is designed to enable efficient trading between fyTokens and their underlying assets. You can read more about it in our [YieldSpace Whitepaper](https://yield.is/YieldSpace.pdf) to get a deeper understanding of the calculations and the overall mechanism. The Yield App integrates YieldSpace seamlessly into the user experience.

### Math64x64

YieldSpace uses ABDK's Math64x64 library, which we have upgraded to 0.8.

### YieldMath

YieldMath.sol uses Math64x64 to implement YieldSpace as described in the whitepaper. The functions implemented in YieldMath are buyBase, sellBase, buyFYToken and sellFYToken.

## Pool

The Pool contracts are a DEX implementation very much in the style of Uniswap v2, but using YieldMath to calculate the trades.

### Liquidity

Each Pool must keep a balance of the two assets it trades: One underlying asset and one fyToken. The marginal interest rate currently offered by the Pool is determined from the relative levels of the reserves of the underlying asset and the fyToken.

To build up these balances, liquidity providers can provide assets using `mint`, receiving pool tokens in exchange. Pool token holders can `burn` them to receive their assets back, plus a proportion of the pool growth that can be attributed to trading fees.

When providing liquidity, assets must be provided in the same proportion as the pool balances, thus a liquidity provider is required to provide precise amounts of underlying and fyToken. When a Pool has exactly zero fyToken, only underlying is provided to `mint`. This is the state when a Pool is first deployed. The initial fyToken balance must come from someone selling fyToken to the Pool.

In a Pool, the fyToken balances must always be higher than the underlying balances for the curve not to go into negative territory. This means that there are always some fyTokens that cannot be purchased. An optimization already present in Yield v1 is that the supply of pool tokens is added to the fyToken balance when trading. These “virtual fyTokens”  allow us to safely reduce the amount of fyToken that must be kept in the Pools.

<!-- TODO: Expand upon how virtual fyTokens are exactly added and when. - Sanket -->

Since providing both underlying and fyToken is inconvenient, the Pool implements a `mintWithBase` function that allows the liquidity provider to provide only underlying, and a virtual trade will be done to convert a proportion of the underlying to fyToken inside the same pool. The amount of underlying to trade for fyToken must be calculated off-chain.

In a similar vein, a `burnForBase` function allows liquidity providers to burn their pool tokens, and trade the fyToken received for underlying without any additional asset transfers, saving gas.

### Trading

The Pool allows the user to sell assets (underlying or fyToken). In this trade, the amount being sold is known, but not the amount being received. Within the same transaction, `sellBasePreview` or `sellFYTokenPreview` can be called to know the exact amount to be received.

The Pool also allows the user to buy assets. In this trade, the amount being received is known, but not the amount taken by the pool. Within the same transaction, `buyBasePreview` or `.mdbuyFYTokenPreview` can be called to know the exact amount to be taken by the Pool.

It is important to note that the Pool doesn’t directly take any assets from anywhere. The assets must be in the Pool before the call to a trading function for the trade to happen. In the case of calls to `buyBase` or `buyFYToken`, where the input amount is not known, the user must either call the appropriate `Preview` function in the same transaction, to transfer the exact amount to the Pool, or must transfer an amount judged sufficient for the trade to execute, and can call `retrieveBase` or `retrieveFYToken` afterwards (but in the same transaction) to recover any surplus.

### TS, G1, and G2

The TS, G1, and G2 parameters determine the trading curve and the trading fees and can be set by the PoolFactory owner via the `setParameter` function for newly deployed Pools.

### Time-Weighted Average Rate

Much like Uniswap v2 implements a Time-Weighted Average Price, Yield v2 implements a Time-Weighted Average Rate following the same pattern. Pool balances are stored along with the time that the TWAR was last updated, and the ratio of the cumulative balance. With this, the Pool can be used by external contracts as an Interest Rate Oracle between an underlying and a fyToken.


## PoolFactory

The PoolFactory is a permissioned contract that allows deploying Pools using CREATE2.


## PoolExtensions

The PoolExtensions library includes functions to calculate the trading limits and invariant for a Pool. A PoolExtensionsWrapper is included to be able to call them externally.


# Strategy

The Strategy contracts allow liquidity providers to pool liquidity to the Strategy, which in turn provides liquidity to a YieldSpace Pool. Upon maturity, the Strategy rolls the liquidity to a new Pool at no cost to liquidity providers.

Liquidity providers mint Strategy tokens with Pool tokens of the pool that the Strategy is currently invested in, and receive a share of Strategy tokens proportional to their investment.

Liquidity providers can burn Strategy tokens of a proportional share of Pool tokens from the Strategy. These Pool tokens are from the current pool, not necessarily the same that they used to invest.

If the Strategy is not invested in any Pool, liquidity providers can burn their Strategy tokens for a proportional share of the underlying held by the Strategy.

A governance action sets the next pool to roll to. This action should be taken by a governor role through a long-delay Timelock, to give investors time to react in case they disagree with the next pool.

A permissioned `startPool` function can be called if the Strategy is not currently investing in any pool, to invest in the next pool set by `setNextPool`.

After the current pool matures, anyone can call `endPool` to convert all the Strategy holdings into underlying.

# Utility Contracts

The Yield v2 protocol doesn't directly reuse any other smart contract implementations. Any general use smart contracts needed were reimplemented and stored in the yield-utils-v2 repository.

## AccessControl

The access control contract was adapted from OpenZeppelin's AccessControl.sol and is inherited from most other contracts in the Yield Protocol.

A role exists implicitly for each function in a contract, with the ROOT role as the admin for the role.

If the `auth` modifier is present in a function, access must have been granted to the caller by an account with the admin role for the function role. This admin role will usually be ROOT, but that can be changed.

An `admin` modifier exists to restrict functions to accounts bearing the `admin` role of a given other role. This is not used outside AccessControl.sol.

An account belonging to the admin role for a function can grant and revoke memberships to the function role.

The ROOT role is special in that it is its own admin so that any member of ROOT can grant and revoke ROOT permissions on other accounts.

There is a special LOCK role, that is also an admin of itself, but has no members. By changing the admin role of a function role to LOCK, no further changes can ever be done to the function role membership, except users voluntarily renouncing to the function role.


## ERC20 and ERC20Permit

The Yield Protocol ERC20 contracts borrow heavily from the DS-Token implementation. The ERC2162 extension is taken from WETH10, which was taken in turn from Yield v1.


## Timelock

The Yield Protocol uses its own implementation of a Timelock, derived from Compound’s original contract, but inheriting from AccessControl and implementing a different pattern to set the earliest time that an execution can be done.


## EmergencyBrake

The EmergencyBrake stores the instructions to remove the orchestration between contracts, which is intended to be used in emergency situations to easily isolate parts of the protocol.
