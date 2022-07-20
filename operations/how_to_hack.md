​# How to Hack the Yield Protocol

This document lists ways in which the Yield Protocol could be hacked, if an error is made. This information will be relevant in hardening the platform and our processes.

When stating the effect from misusing a governance function, only the worst possible outcome is detailed.

This is a living document, which will be updated as we learn of new ways in which we can mess up, or make specific errors impossible.

## Yield-Utils

### Non-revoking of ROOT
All of our permissioned contracts inherit from [AccessControl.sol](https://github.com/yieldprotocol/yield-utils-v2/blob/main/contracts/access/AccessControl.sol). In the constructor, ROOT permission is given to `msg.sender`, which has then to be removed manually. Failing to do so gives ROOT access to the deployer on the deployed contract.

### Accidental granting of ROOT
The ROOT signature is `0x00000000`, or `bytes4(0)`. A call to `grantRole` where the parameter is accidentally set to zero will grant ROOT to `account`.

### Cast malfunction
All of our casting operations are in the [yield-utils-v2 libraries](). An error there would not be clearly visible, and the contracts themselves are not tested. A malfunction would lead to an over/underflow elsewhere.

### WMath malfunction
Fixed math operations are included in our [math libraries](https://github.com/yieldprotocol/yield-utils-v2/tree/main/contracts/math), which are untested to the exception of WPow.sol. An error would lead to unpredictable results anywhere where math happens.

### ERC20 burn on user
The standard `_burn` function on ERC20 doesn't check for ownership or allowance. A careless implementation could lead to scenarios where users' tokens can burn without permission.

### EmergencyBrake DoS via Governance
A governance attack on EmergencyBrake would allow the attacker to remove contract orchestration in the Yield Protocol, leading to an extended Denial of Service

### Timelock attack
Any attack that successfully takes control of the Timelock as a developer would be harmless, but on the other hand taking control of the governor role would be fatal.

### ERC20Rewards Rewards Fat-Fingering
An error in the `end` parameter could make the rewards to go on forever, with no means of cancelling the rewards program.

## Vault

### Join with Rebasing Tokens
With a rebasing token, the result of `token.balanceOf(user)` changes depending on the block, without any transfers needed. If such a token would be onboarded to the Yield Protocol there might be a constantly increasing `token.balanceOf(address(this))`, and its difference to `storedBalance` could be drained through a contract with access to `exit`, such as the Ladle.

### Join with Double Contract Tokens
It is possible to implement a token contract that is permissioned to forward all calls to a second token contract. The result would be that a token contract would have two addresses. If such a contract would be onboarded to Yield Protocol, all assets could be drained calling `retrieve` with the second token address.

### Join Governance Capture
If `exit` access is obtained on a Join, all funds can be immediately drained.

### Oracle Fat-Fingering
There is no verification of inputs on `setSource`. Entering the wrong data could potentially cause wildly inaccurate oracle readings, leading to taking undercollateralized loans and draining of the protocol.

### Accumulator Fat-Fingering
Setting the wrong rate in the Accumulator Oracle would lead to incorrect after-maturity lending or borrowing rates.

### Oracle Wrapper Manipulation
A number of our oracles (Yield Vault, Euler, Lido) feed on exchange rates poorly understood by us. Manipulation of the rates in one of these third-party contracts could potentially cause wildly inaccurate oracle readings, leading to taking undercollateralized loans and draining of the pools.

### FYToken Redemption Freeze on Faulty Oracle
If the chi oracle returns zero in the `_mature` call, subsequent calls to `_accrual`, including `redeem`, will revert due a division by zero error.

### FYToken Pointing to New Oracle After Maturity
If a fyToken is reconfigured to point at a new oracle after maturity, `chiAfterMaturity` will already have been set, and the results could be inconsistent.

### FYToken Governance Capture
If `mint` access is captured an attacker will be able to mint an arbitrary number of fyToken, tradeable in a Pool for assets.

If `burn` access is captured an attacker will be able to burn any tokens approved to the fyToken contract.

If `point` access is captured an attacker would be able to connect the fyToken contract to a Join with a more valuable token per wei (USDC, for example), and redeem their fyToken for more than they are worth, potentially draining the pointed at Join.

### Cauldron Fat-Fingering
`addAsset`: AssetId lost forever.
`setDebtLimits`: Impossible to borrow, dust vaults allowed, no ceiling.
`setLendingOracle`: Debt after maturity could be too high, liquidating users.
`setSpotOracle`: Incorrect price feeds, could lead to uncollateralized borrowing.
`addSeries`: SeriesId lost forever.
`addIlks`: Allowing unsafe collateral for a given underlying, innocuous unless done in conjunction with `setDebtLimits`.

### Cauldron Governance Capture
Depending on the specific function captured, but probably leading to drainage of the whole protocol.

### Ladle Fat-Fingering
`addIntegration`: Features disabled.
`addToken`: Features disabled.
`addJoin`: Difficult to break, even using a malicious Join.
`addPool`: Difficult to break, even using a malicious FYToken.
`addModule`: Unpredictable results, most likely a revert on involved features.
`setFee`: Borrowing disabled due to excessive fees

### Ladle Misuse
Improperly configured batches can lead to the loss of any assets the calling user might hold in the protocol, as well as any assets in his wallet that he would approve the Ladle or Joins for.

### Ladle Governance Capture
Through `addModule` the Ladle can obtain arbitrary new features, which can be used to drain all assets from the Yield Protocol.

### Witch Fat-Fingering
 `point`: Would disable liquidations as all payments fail.
 `setLine`: Proportion being set to zero or at a very small number would disable liquidations. Setting a `line` for a non-existing pair might mean that the real pair is not set to be liquidable.
 `setLimit`: Setting a `limit` for a non-existing pair might mean that the real pair is not set to be liquidable. Setting `max` to zero would only allow one concurrent auction. Setting `max` to 2^128-1 would allow any amount of colalteral to be auctioned concurrently.
 `setAnotherWitch`: It can be misused to protect specific users from liquidation.
 `setIgnoredPair`: It can be misused to disable liquidations for any pair.
 `setAuctioneerReward`: It can be misused to direct all profit for auctioneers, effectively disincentivizing liquidators to liquidate.

### Witch Governance Capture
Disable auctions, leading to protocol insolvency in a downturn. There might be some potential for an attack in which non-liquidable pairs are made liquidable.

### Giver Fat-Fingering
Error on `banIlk`, potentially leading to loss of funds if allowing to give vaults for ilks such as cvx3CRV.

### Giver Governance Capture
Draining of all users overcollateralization, with subsequent liquidation.

## Strategy

### Strategy Fat-Fingering
`setTokenId`: Not sure how to break it.
`setNextPool`: There are safeguards to protect against setting an incorrect pool. The worse case scenario would be a mistake where the maturity of the pool is so far in the future that the Strategy never divests.
`startPool`: If investing on an ongoing pool, setting the wrong ratio guards can lead to sandwiching and loss of funds.

### Strategy Governance Capture
After maturity, a malicious pool can be set to drain all assets from the strategy.

## YieldSpace-TV

### Pool Fat-Fingering
`init`: Loss of initial LP tokens
`setFees`: Unfavourable fees for traders or LPs

### Pool Governance Capture
Fees can be set to 100%, causing loss of funds for unsuspecting traders.

### Pool Front-Running
Pool operations can be front-ran, causing losses for traders and LPs. Slippage guards are provided.

### Pool With Rebasing Tokens
If a rebasing token is used as `shares`, the growth can be drained with `retrieveShares` if positive, and the Pool will brick on `_unwrap` if negative.

### Pool Cache Overestimate
If the fyToken cached balance grows over the supply, the Pool will brick at `_mint` and `_burn`.
If the shares cached balance grows over the real balance, the Pool will revert if minting with no fyToken reserves.
If the fyToken cached balance grows over the real fyToken balance the Pool will revert on regular mints.

### PoolOracle
TBA
