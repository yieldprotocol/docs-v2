# Introduction

Yield Protocol brings fixed-rate borrowing and lending for fixed terms to decentralized finance.

Today, most of the popular decentralized finance protocols are floating-rate. While floating-rate lending and borrowing is a powerful tool, it comes with significant drawbacks. These protocols may experience interest rate volatility that can make it difficult for you to plan for the future, make investment decisions, and properly hedge risk when borrowing and lending. Yield Protocol solves these challenges by introducing fixed-rate, fixed-term borrowing and lending.

In this documentation, you can learn more about how the Yield Protocol works, how to get started, and how to learn more.

<!-- TODO: maybe we can create a blog post outlining use cases and link it here -->

## About

Yield Protocol is an Ethereum-based protocol for collateralized fixed-rate, fixed-term borrowing and lending.

To achieve its goals, Yield uses a class of tokens called *fyTokens* (fixed yield tokens). fyTokens are Ethereum based ERC-20 tokens that can be redeemed for an underlying asset one-to-one after a predetermined maturity date. For example, if you have one fyDai token, you can redeem it for one Dai after the maturity date.

<figure class="image" align = "center">
  <img src="assets/mature.png" width="400" alt="fyDai at maturity" title="fyDai at maturity">
</figure>

fyTokens do not pay interest but instead trade at a discount to their redemption value (like a [zero-coupon bond](https://www.investopedia.com/terms/z/zero-couponbond.asp)), rendering a profit at maturity when it is redeemed for its full face value. The interest rate may be calculated from the difference between the discounted value and the underlying asset's value at maturity.

You can learn more about fyTokens by reading the original [Yield Protocol whitepaper](https://yieldprotocol.com/Yield.pdf).

### An Example

Suppose you buy 1 fyDai that settles exactly a year from today for 0.95 Dai. Your yield is fixed because you have a fixed amount of invested capital (0.95 Dai) and a known amount of future return (1 Dai, a year from now).

A zero coupon bond's price is calculated by the following formula, where **_P_** is the price of the bond, **_M_** is the value of the underlying at maturity, **_r_** is the interest rate and **_n_** is the number of years to maturity.

$$ P = \frac{M}{(1 + r)^n} $$

Plugging our values in the formula and solving for r gives us our interest rate:

$$ 0.95 = \frac{1}{(1 + r)^1} \leftrightarrow r = \frac{1}{0.95} - 1 = 0.0526 $$

### Next Steps

Now that you know how zero coupon bonds work, dive into the [Users](users/) section if you want to learn how to use Yield Protocol, or into the [Developers](developers/) if you are a developer looking to integrate with our smart contracts

## Audit

Yield Protocol was audited by [Code 423n4](https://code423n4.com) and Trail of Bits. You may find the Code 423n4 reports [here](https://code423n4.com/reports/2021-05-yield/) and [here](https://code423n4.com/reports/2021-08-yield/), and the Trail of Bits report [here](https://github.com/trailofbits/publications/blob/master/reviews/YieldV2.pdf).

## Community

Users and the development team are usually in the [Discord server](http://discord.gg/JAFfDj5).

## Bug Bounty

We are offering bounties for bugs disclosed to us at [immunefi.com](https://immunefi.com/bounty/yieldprotocol). The bounty reward is up to $250,000, depending on severity. Please include full details of the vulnerability and steps/code to reproduce. We ask that you permit us time to review and remediate any findings before public disclosure.

Known bugs are listed in [this public repo](https://github.com/yieldprotocol/bugs/issues).

### Contributing

If you have a contribution to make, please reach us out on Discord and we will consider it for a future release or product.

## Governance

Yield Protocol does not currently have a governance token. Currently, most of the maintenance and development of the protocol is performed by the founding development team. The development team is committed to making Yield Protocol a community-owned, community-run protocol by progressive decentralization. If you'd like to help shape the future of the protocol, please get involved by joining our [community](#Community).

## We Are Hiring

You can check out our open roles [here](https://yield-protocol.breezy.hr/).

## Useful Links

> [Yield Dapp](https://app.yieldprotocol.com/)<br> > [Yield Docs](https://docs.yieldprotocol.com/)<br> > [Yield Whitepaper](https://yieldprotocol.com/Yield.pdf)<br> > [YieldSpace AMM Whitepaper](https://yieldprotocol.com/YieldSpace.pdf)<br> > [Github](https://github.com/yieldprotocol)<br> > [Discord Sever](http://discord.gg/JAFfDj5)<br> > [Twitter](https://twitter.com/yield)<br> > [Medium](https://medium.com/yield-protocol)<br>

## Info on these Docs

You can run this documentation offline by using [Docsify](https://docsify.js.org/#/). Fork this repo, [install Docsify](https://docsify.js.org/#/quickstart) on your local machine, and then in the root folder of this repo, type `docsify serve`. The website will be served on port 3000 on your localhost: `localhost:3000`.

[Edit this page](https://github.com/yieldprotocol/docs-v2/edit/main/README.md)
