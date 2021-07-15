# YieldSpace Contracts

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

The Pool also allows the user to buy assets. In this trade, the amount being received is known, but not the amount taken by the pool. Within the same transaction, `buyBasePreview` or `buyFYTokenPreview` can be called to know the exact amount to be taken by the Pool.

It is important to note that the Pool doesn’t directly take any assets from anywhere. The assets must be in the Pool before the call to a trading function for the trade to happen. In the case of calls to `buyBase` or `buyFYToken`, where the input amount is not known, the user must either call the appropriate `Preview` function in the same transaction, to transfer the exact amount to the Pool, or must transfer an amount judged sufficient for the trade to execute, and can call `retrieveBase` or `retrieveFYToken` afterwards (but in the same transaction) to recover any surplus.

### K, G1, and G2

The K, G1, and G2 parameters determine the trading curve and the trading fees and can be set by the Pool owner via the `setParameter` function.

### Time-Weighted Average Rate

Much like Uniswap v2 implements a Time-Weighted Average Price, Yield v2 implements a Time-Weighted Average Rate following the same pattern. Pool balances are stored along with the time that the TWAR was last updated, and the ratio of the cumulative balance. With this, the Pool can be used by external contracts as an Interest Rate Oracle between an underlying and a fyToken.

## PoolFactory

The Pool Factory deploys YieldSpace Pools using CREATE2.

## PoolRouter

The Pool Router batches calls to Pools, along with wrapping/unwrapping of Ether, management of off-chain approvals, and transfers of tokens from users to pools to kickstart transactions.

- Transfer to Pool: Move assets from a user to a Pool
- Route: Execute a function on a Pool
- Permit management: Execute a Dai or ERC2162 off-chain permit.
- Ether management: Convert to WETH, or from WETH.
- Batch: Group any other PoolRouter actions in a single transaction.
