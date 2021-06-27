# Developer Documentation

The Yield Protocol is divided into two repositories, one for the collateralized debt engine called vaults and one for the automated market maker called YieldSpace.

At the core of Yield are user-owned **Vaults** representing a collateralized debt position. Each vault is associated with single collateral and debt in a single series. A series represents a single borrowable asset with a defined maturity date. For example, Alice may own a vault with ETH collateral and debt in the USDC0925 series. The  USDC0925 series represents an obligation to repay USDC on September 25th, 2021.

At the core of Yield Protocol is the **Cauldron**, a smart contract that records the collateral and debt for users. Collateral and debt are kept in vaults that may be owned by a particular address. Cauldron permits management of the full lifecycle of a vault, from creating a vault, adding and removing collateral, adding and removing debt, checking collateralization, permitting liquidation of undercollateralized vaults, and rolling collateral and debt to a new series.

Debt in Yield is tokenized in the form of fyTokens (“fixed yield tokens”). fyTokens are Ethereum based ERC-20 tokens that can be redeemed for an underlying asset one-to-one after a predetermined maturity date. For example, fyUSDC0925 tokens are redeemable for USDC after September 25th, 2021. Each fyToken maturity has an associated **FyToken** ERC-20 smart contract.

Each type of collateral in Yield may be sequestered into a holding contract or **Join** for that collateral. Join contracts receive assets from users and store it until it is removed from the system. Join does not record any vault state. Join permits users to flash borrow all assets it holds.

User activity in Yield is orchestrated by the **Ladle** contract. High-level actions in Yield Protocol are performed by composing smaller discrete actions in the protocol into batches that can be executed in single transactions. The Ladle contract provides a mechanism for batching transactions called a **batch**. Each batching transaction function takes a set of batched actions and calls Ladle member functions associated with each action.

To ensure that vaults are properly collateralized, Yield has various **Oracle** contracts. These contracts wrap external oracles, like Chainlink or Uniswap TWAP oracles.

If users fail to maintain the appropriate level of collateral, they may be liquidated via **Witch**, Yield’s liquidation engine. Witch permits anyone to kick off the liquidation process for an undercollateralized vault. Liquidation is performed via an increasing price auction that increases the amount of collateral the system is willing to pay to cover the debt.

Yield includes an internal AMM to provide liquidity for fyTokens. The AMM is called YieldSpace. YieldSpace is implemented in the **Pool** contract. A **PoolRouter** contract manages calls to and from the pool. YieldSpace pools are designed to trade in interest rates space. That means that the pool will quote trades at the same marginal interest rate over time, absent any trading activity (that would change the interest rate).