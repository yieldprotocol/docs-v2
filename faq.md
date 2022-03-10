# Frequently Asked Questions

If you can't find the answer to your question here, please ask us in [Discord](https://discord.com/channels/752978124614008945).

## General

### Why Yield Protocol?

The goal of the Yield Protocol is to make fixed-rate borrowing and lending a fundamental part of decentralized finance. We believe that fixed-rate is essential to make it possible for defi to onboard its first billion users.

### What differentiates Yield Protocol from other lending protocols?

Yield pioneered fixed-rate, fixed-term borrowing and lending. In our [whitepaper](https://yield.is/Yield.pdf), we were the first to show how fixed-rate, fixed-term borrowing and lending can be achieved by tokenizing loans that are analogous to zero-coupon bonds. We were also the first to create an automated market maker for tokenized loans in our [YieldSpace paper](https://yield.is/YieldSpace.pdf).

### Wen Token?

Yield does not currently have a token, nor are there plans to release one. As a founding team, we believe in decentralization and building towards community control of the protocol. Right now, we believe that is best served by building, and growing community involvement. Long term we expect Yield Protocol to be community-owned and community controlled through a process of progressive decentralization.

### What are fyTokens?

fyTokens are building blocks of Yield Protocol and they essentially represent tokenized loans.

More concretely, fyTokens are Ethereum based ERC20 tokens that can be redeemed for an underlying asset one-to-one after a predetermined maturity date. fyTokens are analogous to zero-coupon bonds in the sense that they do not pay interest but instead trades at a discount, rendering a profit at maturity when it is redeemed for its full face value. The interest rate is calculated by the difference between the discounted value and the underlying asset's value at maturity. For example, if you have one fyDai token, you can redeem it for one Dai after the maturity date.

### What assets can be borrowed or lent?

Yield Protocol allows multiple assets to be borrowed or lent at a fixed rate.

### What assets can be used as collateral?

Yield Protocol supports multiple assets as collateral to borrow multiple assets at fixed rate. You can view the available collateral at: [Collateral Page]().

### How can I find you on uniswap?

Yield does not have a governance token yet. You will not find us on Uniswap and you should be wary about any token claiming to be from Yield Protocol.

<!-- ### Would there be an auto-roll feature? -->

### Does the Yield Protocol have protocol fees?

At the moment, no.

To ensure that the Yield Protocol has a path towards revenue for future development, v2 includes the ability to charge borrowers origination fees. At launch, these fees will be disabled. The decision to turn on the fees and at what levels are reserved for a future determination by the community.

### When Layer2?

We are currently live on Arbitrum! Visit us at [app.yieldprotocol.com](https://app.yieldprotocol.com/) to get started. We will continue to evaluate other L2's in the future. If you'd like us to implement another L2, let us know on [Discord](https://discord.com/channels/752978124614008945/897303879400689664)!

### When was Yield launched?

Yield was incubated at [Paradigm](https://www.paradigm.xyz) and the first version went live on 19 Oct 2020.

<!-- ### Where does the fixed yield come from?
From borrowers. Determined by the market supply and demand. -->

## Borrowing

### How to borrow?

Visit us at [app.yieldprotocol.com](https://app.yieldprotocol.com/) to get started. You will need an Ethereum wallet(like [Metamask](https://metamask.io)), some ETH to pay for transaction fees and the base asset you would like to use as collateral. Once you have these things, you can choose the series from the app according to your needs and start the borrowing process.

### How much collateral is required for borrowing?

The amount of collateral required depends on the collateral being provided and the underlying asset pair.

### Do we earn interest on the deposited collateral?

Yield does not currently pay interest on deposited collateral.

### What is a Vault?

A vault is a collateralized debt position. A user may have many vaults. Each vault permits the deposit of one type of collateral and permits borrowing a single asset for a fixed-term.

### What is a Series?

A series represents a single borrowable asset with a defined maturity date. Each series corresponds to a single ERC-20 fyToken.

### Can anyone set up a new series?

No. Series are created by Yield governance.

### What happens if I don't close the loan at the time of maturity?

Loans held open past maturity accrue fees at a floating-rate APR. You may pay back the loan at any time to retrieve your collateral. Accumulated interest on your loan counts towards your debt, so you should monitor your loans to ensure that they are properly collateralized at all times.

### Borrowing and Lending rates look identical to me? How is that sustainable?

For very small amounts the rates are very similar but as the trade grows in size the rates move away from each other.

### What collateralization ratio should I choose to avoid liquidation?

It depends on how much risk you want to take and how actively you plan to manage your positions. The lower your collateral ratio, the greater your risk of liquidation. If you don’t need to worry about getting liquidated and only need to top up your balance if there is a significant market decline then you can choose your collateralization ratio in the range of 250%-300%. If you know what you are doing and want to get as much leverage as possible out of your collateral then you can lower your collateralization ratio according to your needs.

## Lending

### How does lending work in Yield?

Lenders can lend at a fixed-rate by purchasing fyTokens. fyTokens can be held until the maturity date, upon which they may be redeemed for principal and interest.

### Can I purchase fyTokens directly?

Yes! Purchasing fyTokens directly is equivalent to lending.

### Why is my current value as shown less than the amount I lent?

When you lend, you are selling underlying asset into the pool and getting fyTokens. When you close your position you are selling the fyToken and getting your underlying asset back. Both operations in the pool involve paying a fee of approximately 5% of the interest paid. The current value reflects the return on selling the fyToken in the pool, which reflects the trading fee, interest rate changes and slippage.

### How do interest rate changes affect me as a lender?

Lenders are effectively holding fyTokens. If interest rates go up, the value of lenders’ holdings goes down. However, if you hold on to it until maturity, you will always receive your principal plus interest at the rate locked in when buying the fyTokens.

### If I lend money for a particular series, does that mean my amount is locked for the duration of that series?

No. You can sell the lending position early, but doing so may impact your returns and may even cause loss of principal.

### How do you calculate the portfolio value?

Portfolio value at maturity is the fyToken balance which represents the amount of underlying asset that the fyToken can be redeemed for at maturity.

## Providing Liquidity

### What's the difference between pool and lend?

You lend at a fixed rate. When you pool, you provide liquidity for both borrowing and lending. The returns to pooling depend on the fees earned by the pool and the path taken by interest rates. Liquidity providers may also earn interest from fyTokens held by the liquidity pool.

### What is a liquidity strategy?

All liquidity providing through the Yield app is done through liquidity strategies. Liquidity Strategies are designed to provide liquidity to pools on your behalf and move the liquidity to new pools when your current pool expires after maturity.

### Why would I want to be a Liquidity Provider?

Liquidity Providers earn fees from users trading. Thus, liquidity providers can make returns in any interest rate environment.

### Is there any incentive or reward for liquidity providers besides trading fees?

There are currently no yield farming incentives or other token rewards in Yield. All returns are from fees paid by users.

### As a liquidity provider is the yield for all series the same?

No, liquidity providers earn fees from trading in that series between fyTokens and underlying tokens, so the yield is dependent upon trading volume for each series.

### How do interest rates and fyToken prices relate to each other?

They are inversely correlated. If the fyToken price for a series increases, the interest rate for that series goes down.

### Is lending the same thing as providing liquidity?

No! Lenders earn fixed-rate interest rates whereas liquidity providers' return depend on the fees earned by the pool and the path taken by interest rates.

## Liquidation

### What is Liquidation?

Borrowers must maintain a minimum amount of collateral in their vault to secure the debt they owe. If a borrower fails to do so, their vault may be liquidated. Their collateral will be seized and auctioned off to repay their debts.

### How does liquidation process work?

When the value of the collateral in a borrowing position becomes less than the value of the debt times the collateralization ratio, the position will be put up for auction. Liquidators will repay the debt in exchange for the collateral until there is no debt left. The borrowing position (vault) will be returned to the original owner with any collateral left after the liquidators have repaid all the debt.

### Who liquidates positions?

Anyone may liquidate an insufficiently collateralized borrowing position.

## Security

### Is Yield Protocol audited?

Yes! Yield Protocol was audited by [Code 423n4](https://code423n4.com). You may find the report [here]().

### What are the risks involved in using the protocol?

There are two main risks involved in using the protocol: impermanent loss and smart contracts risks.

Impermanent loss happens when you provide liquidity to a liquidity pool, and the price of your deposited assets changes compared to when you deposited them. In Yield Protocol, an impermanent loss is relatively low as it happens only when interest rates change. Interest rates would have to move a lot to make impermanent loss significant. And, as long as you stay in the pool until maturity, you will get all your assets back plus more.

The other risk is that the smart contracts could get hacked. This is a risk of all DeFi protocols, and is not exclusive to Yield. We take security extremely seriously and take every possible measure to ensure that this won't happen. Our system has been audited by [Code 423n4](https://code423n4.com). Audits don't eliminate all risk, but they are the gold standard in checking for smart contract bugs. We are constantly working to catch bugs, and have created a bug bounty program to further lower risk.

### Do you have a bug bounty program?

Yes! We are offering bounties for bugs disclosed to us at [immunefi.com](https://immunefi.com/bounty/yieldprotocol). The bounty reward is up to $250,000, depending on severity. Please include full details of the vulnerability and steps/code to reproduce. We ask that you permit us time to review and remediate any findings before public disclosure.

## Miscellaneous

<!-- ### What is the fee structure? -->

### How is APR calculated?

fyTokens interest rates are determined by the market rate from the appropriate liquidity pool.

### What is impermanent loss?

Impermanent loss refers to a temporary loss caused to a liquidity provider due to the volatility in a trading pair. Impermanent loss happens when you provide liquidity to a liquidity pool, and the price of your deposited assets changes compared to when you deposited them. The bigger this change is, the more you are exposed to impermanent loss. In this case, the loss means less dollar value at the time of withdrawal than at the time of deposit.

### Is it right that you don't have impermanent loss as a Liquidity Provider in Yield?

If you add liquidity to a pool and remove it only after maturity, you won't have a loss. If you remove your liquidity before maturity, your profit or loss will depend on the fees earned by the pool and the path taken by the interest rates.

### Does lending and borrowing happen in one transaction?

Yes!

### What is YieldSpace?

To improve the liquidity of fyTokens, Yield has designed an automated liquidity provider that is designed to enable efficient trading between fyTokens and their underlying assets. We call this new automated liquidity provider YieldSpace. You can read more about it in our [YieldSpace Whitepaper](https://yield.is/YieldSpace.pdf) to get a deeper understanding of the calculations and the overall mechanism. The Yield App integrates YieldSpace seamlessly into the user experience.

### What is a Flash Loan?

A loan that requires no collateral from the borrower, but that needs to be repaid within the same transaction. They are commonly used for arbitrage or refinancing.

### How is Yield Governed?

The founding team believes in decentralization and building towards community control of the protocol. Right now, we believe that is best served by building, and growing community involvement. Long term we expect Yield Protocol to be community-owned and community controlled through a process of progressive decentralization.
