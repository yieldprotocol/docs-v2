# Users

## Getting Started

The Yield App is available at [https://app.yield.is](https://app.yield.is/).

Using the Yield App, you can borrow or lend assets at a fixed rate for a fixed term. You may also use the Yield app to provide liquidity to earn fees from other users looking to borrow and lend.

At the core of Yield are user-owned vaults representing a collateralized debt position. Each vault is associated with single collateral and debt in a single series.

A series represents a single borrowable asset with a defined maturity date. Each series corresponds to a single ERC-20 fyToken. For example, Alice may own a vault with ETH collateral and debt in the USDC0925 series. The USDC0925 series represents an obligation to repay USDC on September 25th, 2021.

## Borrowing

To borrow, you deposit collateral in the protocol and draw new fyTokens against the collateral. You may proceed to sell the fyTokens for the underlying token, locking in your borrowing rate. Yield Protocol has a built-in automated market maker(AMM) called YieldSpace to enable efficient selling of fyTokens.

// which happens automatically behind the scenes in our app - Sanket

At maturity, you must repay the debt to reclaim your collateral. Of course, you may also repay your debt earlier than the maturity by returning the fyTokens you have drawn. Interest rate changes may affect (positively or negatively) the amount of the borrowed asset you need to spend to obtain the needed fyTokens. Be careful when repaying earlier as you may incur higher interest rates than paying at maturity.

After maturity, if you don't close your borrowing position, floating-rate interest will be charged to keep the position open.

// need to expand more as how floating-rate interest is calculated after maturity - Sanket

## Lending

To lend at a fixed rate, you simply have to buy fyTokens from our YieldSpace AMM at a discount.

The discount you receive from the face value of the fyToken locks in a fixed return that can be calculated based on the time to maturity.

For example, if on September 31, 2021, you buy 100 fyDAI that matures in December 2021 for 98.8 DAI you will earn an implied rate of interest of 5% APR.

fyTokens can be held until the maturity date, upon which they may be redeemed for principal plus interest.

You can also exit your lending position early by selling your fyToken for an underlying asset. Because fyTokens are traded freely, changes in interest rates may affect the amount of underlying assets you receive when redeeming early.

To compensate lenders who do not redeem fyTokens right away, after maturity they begin earning interest in the form of an increasing redemption rate.

// need to expand more as how interest paid will be calculated after maturity - Sanket

## Providing Liquidity

While fyTokens are ERC-20 assets and can be traded using existing protocols like Uniswap, those protocols are not optimized for fyTokens. To improve the liquidity of fyTokens, Yield has designed a new automated liquidity provider that is designed to enable efficient trading between fyTokens and their underlying assets. We call this new automated liquidity provider YieldSpace. You can read more about it in our [YieldSpace Whitepaper](https://yield.is/YieldSpace.pdf) to get a deeper understanding of the calculations and the overall mechanism. The Yield App integrates YieldSpace seamlessly into the user experience.

YieldSpace pools improve on existing solutions by providing markets that quote at consistent interest rates over time, in the absence of trades. By quoting at a consistent interest rate, YieldSpace pools minimize losses from arbitrage.

Whereas in Uniswap, arbitrage trades are expected whenever prices change, arbitrage trades in the YieldSpace Pool are expected to occur only when interest rates change. This should tend to reduce the “impermanent loss” suffered by market makers.

Like other automated liquidity providers, users may choose to provide liquidity to YieldSpace pools to earn fees from future trades. YieldSpace uses a custom fee model that is optimized for fyTokens. Rather than charge a fee that is a percentage of the amount of the asset bought or sold, YieldSpace charges a fee that is proportional to both interest rate and time to maturity. This fee model ensures that fees never result in the unreasonable amount on interest rates paid by borrowers (fyToken sellers) and earned by lenders (fyToken buyers).

Similar to Uniswap, providing liquidity to YieldSpace will grant you Liquidity Provider shares. If you are providing liquidity for fyDai21Sep30, you get fyDaiLP21Sep30 tokens which represent your share of the pool. These tokens are ERC-20 tokens and may further be composed in the Defi ecosystem.

Each fyToken has an associated YieldSpace pool that permits trading between that fyToken and its underlying asset. Swapping fyTokens with different maturities needs to be done manually.

## Governance
