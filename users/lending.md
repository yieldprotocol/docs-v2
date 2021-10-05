# Lending

To lend, you first access the "Lend" tab in the [Yield v2 App](https://app.yieldprotocol.com/#/lend).

When you lend in Yield, you are buying future cash payments at a discount. These future cash payments are represented by tokens that we call “fyTokens”. A fyToken is a token that can be redeemed one-for-one for a base asset on some future date. FyTokens don’t pay interest themselves, rather the interest is determined by the difference between the face value of the token and the price you pay for it. 

The fixed interest rate you receive when lending is determined by a built-in automated market, and the more you lend, the lower your interest rate may be. 

## Quick Start

Lending on the Yield Protocol is a very simple process. You choose an asset you want to lend and you review and initiate the transaction. The first step is selecting the base asset that you want to lend. You can lend assets such as Dai, USDC, and USDT. Once you’ve selected the asset, you can indicate the amount of the asset you wish to lend. 

Once you’ve entered an amount, you may select the series you wish to lend. Each series has a different maturity date. Yield currently provides quarterly maturity dates. Something to take into consideration: when selecting a series, you want to select a date that corresponds closely to the time you think you want to close the loan.



## Lending Under the Hood

To lend at a fixed rate, you simply buy fyTokens at a discount to their face value. The discount you receive is equivalent to locking in a fixed return that can be calculated based on the time until a fyToken can be redeemed. 

For example, if on September 31, 2021, you buy 100 fyDai that matures in December 2021 for 98.8 Dai you will earn an implied rate of interest of 5% APR.

fyTokens can be held until the maturity date, upon which they may be redeemed for principal plus interest.

You can also exit your lending position early by selling your fyToken for an underlying asset. Because fyTokens are traded freely, changes in interest rates may affect the amount of underlying assets you receive when redeeming early.

To compensate lenders who do not redeem fyTokens right away, after maturity they begin earning interest in the form of an increasing redemption rate.

 <!-- TODO: need to expand more as how interest paid will be calculated after maturity. -->

#### Maturity

If you own fyDai after maturity, you will start to earn the Dai Savings Rate on your fyDai, until you decide to redeem it for Dai.

 ![](../assets/redemption.png)