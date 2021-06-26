# Frequently Asked Questions

If you can't find the answer to your question here, please ask us in [Discord](https://discord.com/channels/752978124614008945).

## General

### Why Yield Protocol?

The goal of the Yield Protocol is to bring fixed-term, fixed-rate borrowing and lending to decentralized finance. Today most of the popular decentralized finance protocols are floating-rate. While floating-rate lending/borrowing is a powerful tool, it comes with significant drawbacks. Decentralized finance has several use cases that can be greatly improved with fixed-rate, fixed-term borrowing, and lending.

### What differentiates Yield Protocol from other lending protocols?

Yield pioneered fixed-rate, fixed-term borrowing and lending.

### Wen Token?

Yield does not currently have a token, nor are there plans to release one. As a team, we believe in decentralization and building towards community control of the protocol. Right now, we believe that is best served by building, and growing community involvement. 

### What are fyTokens?

fyTokens are essentially tokenized loans. 

More concretely, fyTokens are Ethereum based ERC20 tokens that can be redeemed for an underlying asset one-to-one after a predetermined maturity date. fyTokens are analogous to zero-coupon bonds in the sense that they do not pay interest but instead trades at a discount, rendering a profit at maturity when it is redeemed for its full face value. The interest rate is calculated by the difference between the discounted value and the underlying asset's value at maturity. For example, if you have one fyDai token, you can redeem it for one Dai after the maturity date.  

### What assets can be borrowed or lent?

Yield Protocol allows multiple assets to be borrowed or lent at a fixed rate.

### What assets can be used as collateral?

Yield Protocol supports multiple assets as collateral to borrow multiple assets at fixed rate. You can view the available collateral at: [[COLLATERAL PAGE]]

### How can I find you on uniswap?

Yield does not have a governance token. You will not find us on Uniswap and you should be wary about any token claiming to be from Yield Protocol.

### Is Yield Protocol audited?

Yes! By Trails of Bits, [here is the report](https://github.com/trailofbits/publications/blob/master/reviews/YieldProtocol.pdf).

// For v2, we are doing a Code 432n4 (Code Arena) hacking contest instead, there will be a report the same as if we would have hired an audit company - Alberto

// Will edit once more details are available - Sanket

### Would there be an auto-roll feature?

// We need to discuss this for v2. There are also three things that can be rolled: Debt, Lending, and Liquidity. - Alberto

// I don't know if this is a "frequently" asked question. Might be better to change to "How can I roll my positions?" - Allan

### Does the Yield Protocol have protocol fees?

At the moment, no. 

To ensure that the Yield Protocol has a path towards revenue for future development, v2 includes the ability to charge borrowers origination fees. At launch, these fees will be disabled. The decision to turn on the fees and at what levels are reserved for a future determination by the community.

### When Layer2?

### Where does the fixed yield come from?

## Borrowing

### How to borrow?

Visit us at [app.yield.is](http://app.yield.is) to get started. 

### How much collateral is required for borrowing?

The amount of collateral required depends on the collateral being provided. You can learn more about Yield's collateral requirements at: 

// For v2, it will depend on the collateral/underlying pair

### Do we earn interest on the deposited collateral?

Yield does pay interest on deposited collateral. 

### What is a Vault?

A vault is a collateralized debt position. A user may have many vaults. Each vault permits the deposit of one type of collateral and permits borrowing a single asset for a fixed-term.

### What is a Series?

A series represents a single borrowable asset with a defined maturity date. Each series corresponds to a single ERC-20 fyToken. 

### Can anyone set up a new series?

No. Series are managed by Yield governance.

### What happens if I don't close the loan at the time of maturity?

Loans held open past maturity accrue fees at a floating-rate APR. You may pay back the loan at any time to retrieve your collateral. Accumulated interest on your loan counts towards your debt, so you should monitor your loans to ensure that they are properly collateralized at all times.   

### Borrowing and Lending rates look identical to me? How is that sustainable?

For very small amounts the rates are very similar but as the trade grows in size the rates move away from each other.

## Lending

### How does lending work in Yield?

Lenders can lend at a fixed-rate by purchasing fyTokens. fyTokens can be held until the maturity date, upon which they may be redeemed for principal and interest. 

### Can I purchase fyTokens directly?

Yes! Purchasing fyTokens directly is equivalent to lending.

### Why is my current value as shown less than the amount I lent?

When you lend, you are selling underlying asset into the pool and getting fyToken. When you close your position you are selling the fyToken and getting your underlying asset back. Both operations in the pool involve paying a fee of approximately 5% of the interest paid. The current value reflects the return on selling the fyToken in the pool, which reflects the trading fee, interest rate changes and slippage.

### How do interest rate changes affect me as a lender?

Lenders are effectively holding fyTokens. If interest rates go up, the value of lenders’ holdings goes down. However, if you hold on to it until maturity, you will always receive your principal plus interest at the rate locked in when buying the fyTokens.

### If I lend money for a particular series, does that mean my amount is locked for the duration of that series?

No. You can sell the lending position early, but doing so may impact your returns and may even cause loss of principal.

### How do you calculate the portfolio value?

Portfolio value at maturity is the fyToken balance which represents the amount of underlying asset that the fyToken can be redeemed for at maturity.

## Providing Liquidity

### What's the difference between pool and lend?

You lend at a fixed rate. When you pool, you provide liquidity for both borrowing and lending. The returns to pooling depend on the fees earned by the pool and the path taken by interest rates.

### Why would I want to be a Liquidity Provider?

Liquidity Providers earn fees from users trading. Thus, liquidity providers can make returns in any interest rate environment. 

### Is there any incentive or reward for liquidity providers besides trading fees?

There are currently no yield farming incentives or other token rewards in Yield. All returns are from fees paid by users.

### As a liquidity provider is the yield for all series the same?

No, liquidity providers earn fees from trading in that series between fyTokens and underlying tokens, so the yield is dependent upon trading volume for each series.

### How do interest rates and fyToken prices relate to each other?

They are inversely correlated. If the fyToken price for a series increases, the interest rate for that series goes down.

## Flashloans

### What is a Flash Loan?

A loan that requires no collateral from the borrower, but that needs to be repaid within the same transaction. They are commonly used for arbitrage or refinancing.

### How does flash loan work?

// Probably too advanced for an FAQ, maybe we just link something here. - Alberto

// I might even remove this entire section. - Sanket

// I think we should remove this section. -Allan

## Liquidation

### What is Liquidation?

Borrowers must maintain a minimum amount of collateral in their vault to secure the debt they owe. If a borrower fails to do so, their vault may be liquidated. Their collateral will be seized and auctioned off to repay their debts.

### How does liquidation process work?

When the value of the collateral in a borrowing position becomes less than the value of the debt times the collateralization ratio, the position will be put up for auction. Liquidators will repay the debt in exchange for the collateral until there is no debt left. The borrowing position (vault) will be returned to the original owner with any collateral left after the liquidators have repaid all the debt. 

### Who liquidates positions?

Anyone may liquidate an insufficiently collateralized borrowing position.

## Miscellaneous

### What is the fee structure?

### How is APR calculated?

### What is impermanent loss?

### Is it right that you don't have impermanent loss as a Liquidity Provider in Yield?

If you add liquidity to a pool and remove it only after maturity, you won't have a loss. If you remove your liquidity before maturity, your profit or loss will depend on the fees earned by the pool and the path taken by the interest rates.

### How much does it cost to use the Yield Protocol?

// This is outdated for v2, we need to come up with sample costs - Alberto

// Removed it! I was unhappy with it anyways because it was intimidating to look at - Sanket

### What is YieldSpace?

To improve the liquidity of fyTokens, Yield has designed an automated liquidity provider that is designed to enable efficient trading between fyTokens and their underlying assets. We call this new automated liquidity provider YieldSpace. You can read more about it in our [YieldSpace Whitepaper](https://yield.is/YieldSpace.pdf) to get a deeper understanding of the calculations and the overall mechanism. The Yield App integrates YieldSpace seamlessly into the user experience.

### What are the possible risks involved in using the app?

### Do you have a bug bounty program?

Yes! We are offering bounties for bugs disclosed to us at [security@yield.is](mailto:security@yield.is). The bounty reward is up to $25,000, depending on severity. Please include full details of the vulnerability and steps/code to reproduce. We ask that you permit us time to review and remediate any findings before public disclosure.

// We should add the Immunefi bug bounty here.  [https://immunefi.com/bounty/yieldprotocol/](https://immunefi.com/bounty/yieldprotocol/)