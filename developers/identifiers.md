# Yield Identifiers

Yield Protocol uses 6 bytes to generate predictable identifiers for assets and series.

## Before 2023

Assets get a sequential number, typed as a string. For example "00" is for ETH, which converted to hexadecimal is 0x303000000000
Series get the first two characters of the asset identifier for their underlying, followed by two digits representing the quarters since the start of 2021. For example, the ETH series maturing on December 2022 is "00" for ETH, followed by "08" since December 2022 is the end of the eight quarter since the start of 2021.

## Starting on 2023

The six bytes of an identifier get divided in 12 hexadecimal characters.

1 - Type of identifier
2:4 - Asset identifier
5:6 - Provider identifier
7:9 - Reserved
10:12 - Iteration 

### Type
0 - Series
1 - Strategy
2 - Reserved
3 - Basic asset, potentially a base
4 - Series from other providers as assets
5:C - Reserved
D - Derivative asset
E:F - Reserved

### Asset
The asset is three hexadecimal characters.

For type 3 (base assets), they are assigned manually by Yield.
For all other types, they are the identifier of the appropriate base asset

### Provider
The provider is two hexadecimal characters.

00 - Unknown / One off / Not set
FF - Yield

### Iteration
For series, number of canonical months between the epoch and maturity.
`iteration = maturity /↓ (30 * 86400)`

For other assets, it can be used as versioning.

### Examples
The naming scheme was designed so the existing basic assets would fit with their current identifiers. Some derivatives have basic identifiers such as wstEth or yvUSDC, which are incorrect but can be remediated by adding those collaterals again. Series, FCash and fyToken as collateral also have incorrect identifiers but those will be eventually phased out and will only take out some of the basic identifiers slots.

```
ETH        = 3 030 00 000 000 (Basic asset, ETH, Not set)
DAI        = 3 031 00 000 000 (Basic asset, DAI, Not set)
USDC       = 3 032 00 000 000 (Basic asset, USDC, Not set)
FRAX       = 3 138 00 000 000 (Basic asset, FRAX, Not set)
FYETHDEC22 = 0 030 FF 285 (Series, ETH, Yield, 645 canonical months since epoch in hexadecimal).
FETHDEC22  = 4 030 10 285 (FCash collateral, ETH, Notional, 645 canonical months since epoch in hexadecimal).
YSDAI6MJD  = 1 031 FF 000 000 (Strategy, DAI, first instance)
YSDAI6MMS  = 1 031 FF 000 001 (Strategy, DAI, second instance)
CRV        = 3 040 11 000 000 (Basic asset, CRV, Curve)
CVX3CRV    = D 040 12 000 000 (Derivartive asset, CRV, Convex)
```

[Edit this page](https://github.com/yieldprotocol/docs-v2/edit/main/developers/addresses.md)