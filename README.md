# Introduction

Yield brings fixed-term, fixed-rate borrowing and lending to decentralized finance.

Today most of the popular decentralized finance protocols are floating-rate. While floating-rate lending and borrowing is a powerful tool, it comes with significant drawbacks. These protocols may experience interest rate volatility that can make it difficult for you to plan for the future, make investment decisions, and properly hedge risk when borrowing and lending. Decentralized finance has several use cases that can be greatly improved with fixed-rate, fixed-term borrowing and lending.

<!-- TODO: maybe we can create a blog post outlining use cases and link it here -->

To enable fixed-rate, fixed-term borrowing and lending, Yield uses a class of tokens called *fyTokens(fixed yield tokens)*. fyTokens are Ethereum based ERC-20 tokens that can be redeemed for an underlying asset one-to-one after a predetermined maturity date. For example, if you have one fyDai token, you can redeem it for one Dai after the maturity date.

<figure class="image" align = "center">
  <img src="assets/mature.png" width="400" alt="fyDai at maturity" title="fyDai at maturity">
</figure>

fyTokens are analogous to [zero-coupon bonds](https://www.investopedia.com/terms/z/zero-couponbond.asp) in the sense that they do not pay interest but instead trade at a discount, rendering a profit at maturity when it is redeemed for its full face value. The interest rate is calculated by the difference between the discounted value and the underlying asset's value at maturity.

### An Example

Suppose you buy 1 fyDai that settles exactly a year from today for 0.95 Dai. Your yield is fixed because you have a fixed amount of invested capital (0.95 Dai) and a known amount of future return (1 Dai, a year from now).

A zero coupon bond's price is calculated by the following formula, where ***P*** is the price of the bond, ***M*** is the value of the underlying at maturity, ***r*** is the interest rate and ***n*** is the number of years to maturity.

$$ P = \frac{M}{(1 + r)^n} $$

Plugging our values in the formula and solving for r gives us our interest rate:

$$ 0.95 = \frac{1}{(1 + r)^1} \leftrightarrow r = \frac{1}{0.95} - 1 = 0.0526 $$

### Next Steps

Now that you know how zero coupon bonds work, dive into the [Users](users/) section if you want to learn how to use Yield Protocol, or into the [Developers](developers/) if you are a developer looking to integrate with our smart contracts

## Contract Addresses

<!-- TODO: Will be available once released on mainnet -->

### Audit
Yield Protocol was audited by [Code 423n4](https://code423n4.com). You may find the report [here]().

## Governance
Currently Yield is governed by the founding team but we are committed to decentralization and will relinquish control of the protocol and give it to the community. 

## Community
Users and the development team are usually in the [Discord server](https://discord.com/channels/752978124614008945).

## Bug Bounty
We are offering bounties for bugs disclosed to us at [immunefi.com](https://immunefi.com/bounty/yieldprotocol). The bounty reward is up to $50,000, depending on severity. Please include full details of the vulnerability and steps/code to reproduce. We ask that you permit us time to review and remediate any findings before public disclosure.

### Contributing
If you have a contribution to make, please reach us out on Discord and we will consider it for a future release or product.

### How To Setup
You can run this documentation offline by using [Docsify](https://docsify.js.org/#/). Fork this repo, [install Docsify](https://docsify.js.org/#/quickstart) on your local machine, and then in the root folder of this repo, type `docsify serve`. The website will be served on port 3000 on your localhost: `localhost:3000`.

## We Are Hiring
You can check out our open roles here on our [careers](https://yield.is/careers) page.

## Useful Links
> [Yield Dapp](https://app.yield.is/)<br>
> [Yield Docs](https://docs.yield.is/)<br>
> [Yield Whitepaper](https://yield.is/yield.pdf)<br>
> [YieldSpace AMM Whitepaper](https://yield.is/yieldspace.pdf)<br>
> [Github](https://github.com/yieldprotocol)<br>
> [Discord Sever](https://discord.com/channels/752978124614008945)<br>
> [Twitter](https://twitter.com/yield)<br>
> [Medium](https://medium.com/yield-protocol)<br>