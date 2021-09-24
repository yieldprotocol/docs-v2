## Introduction


This guide is valid for the 10.3 deployment of Yield v2.


```
   "@yield-protocol/strategy-v2": "^0.10.0-rc0",
   "@yield-protocol/utils-v2": "^2.4.0",
   "@yield-protocol/vault-interfaces": "^2.3.0-rc1",
   "@yield-protocol/vault-v2": "^0.10.0-rc5",
   "@yield-protocol/yieldspace-interfaces": "^2.3.0-rc0",
   "@yield-protocol/yieldspace-v2": "^0.10.0-rc2",
```

The deployment steps are numbered sequentially.


## Permissioning


The Yield v2 contracts have individual access control features for each permissioned function. At its very minimum, two actors will be needed to operate the protocol safely: The **governor** and the **operations team**. The permissions to the protocol of these two actors are channeled through the **Timelock** and the **EmergencyBrake**.


### Actors


**Governor**: The governor is an account with complete control over the protocol, through a timelock. It is expected to be an x-of-n multisig, or a governance contract.


**Operations team**: The operations team has limited control to execute what is determined by governance. The AccessControl contract is implemented as a 1-of-n multisig and so individual members can directly be given permissions consistent with an operations role. In the future, an additional x-of-n multisig could be considered for the operations role.


This is a minimal setup suitable for launch. Over time, as the protocol grows, it is expected that the governor and operations roles will be subdivided and possibly placed in a hierarchy, to reduce the risk profile of any individual account.


### Timelock


The **Timelock** has complete control over the protocol. It has the `ROOT` permission to all contracts, which allows the Timelock to `grant` and `revoke` permissions in all contracts. It also has permission to execute all the governance functions in all contracts. The Timelock can grant itself permission to execute non-governance functions in the contracts as well, even if that is not expected to be a common occurrence.


In the Timelock, the **governor** has permissions to `propose`, `approve` and `execute`. In that sense the governor has complete control over the protocol, subject to the `delay` set in the Timelock, which is the only safety mechanism that protects the protocol after a governance takeover. It’s important to note that a governance takeover could well be fatal for the protocol and must be avoided at all costs.


The governor is not expected to `propose` or `execute` proposals, and has those permissions as a protection against a coordinated attack by the operations team. The governor has no other permissions over the protocol apart from controlling the Timelock.


In the Timelock, the **operations team** has permissions to `propose` and `execute`. The operations team can set up the proposals, and once they are approved by the governor, can execute them at an appropriate time.


### EmergencyBrake


The **EmergencyBrake** has a partial control over the granting and revoking permissions over the entire protocol. It has the `ROOT` permission to all contracts, but its implementation only allows the EmergencyBrake to grant and revoke permissions in a predetermined manner. The EmergencyBrake doesn’t have access to any permissioned function in any contract, it can’t grant permission to itself to any contract, and it is not expected to be granted any permission either.


In the EmergencyBrake, the **Timelock** has the permission to `plan` emergencies, which will usually be set up in the same proposal that orchestrates new contracts. The Timelock also has permission to `restore` or `terminate` orchestrations that have been paused by the execution of a plan.


In the EmergencyBrake, the **operations team** has the permission to `execute` plans, effectively pausing parts of the protocol.


## Configuration


For a protocol deployment, these are the parameters that need to be defined.



* Governor
* Operations team
* Timelock delay
* Assets and assetIds
* Bases
* Ilks for each base, with collateralization ratio and limits for each pair
* For each Ilk: initial offer and duration for the witch
* Series, with seriesId, base, maturity, ilks, name and symbol
* Name and symbol for each pool
* Assets to initialize each pool
* Strategies to deploy, with name and symbol, starting pool and next pool

## Mocks


Often, the operations will involve the addresses of external contracts. These won’t be needed on mainnet.


### Assets and Oracle Sources


For testing purposes we use a series of mock assets and oracle sources. 


```
npx hardhat run --network kovan core/01_mocks.ts
```



**Output**: assets.json, chiSources.json, rateSources.json, spotSources.json


## Core


These steps are executed only once in the life of Yield v2. If they need to be executed again that is considered a **major** change subject to the maximum scrutiny.


### Libraries


There are two external libraries used in the Yield v2 protocol. They are not orchestrated or permissioned, and store no data. They should never need to be redeployed.


```
npx hardhat run --network kovan core/10_libraries.ts
```



### Timelock


During operations the Timelock will allow users to know in advance of any changes to the protocol. During deployment it will allow us to bundle transactions.


```
npx hardhat run --network kovan core/11_timelock.ts
```



The Timelock is deployed with no delay and with all the roles granted to the deployer. This initial configuration makes it work as a transaction relayer. Before the protocol is live, the roles and delay will be adjusted so that the Timelock assumes its intended function.


**Input**: governance.json


**Output**: governance.json


### Relay


To ease operations in testnets, a Relay is deployed that has governance permissions over Timelock. Owning the Relay means being able to propose, approve and execute Timelock proposals in a single transaction. Do not use on mainnet.


```
npx hardhat run --network kovan mocks/02_relay.ts
```



**Input**: governance.json


**Output**: governance.json


### Cloak


The Cloak stores the orchestration permissions between contracts, in a way that it will be safe to isolate contracts from the rest of the protocol in an emergency.


```
npx hardhat run --network kovan core/12_cloak.ts
```



All contracts that inherit from AccessControl must give ROOT access to the Cloak, so that the latter can edit the access control permissions.


**Input**: governance.json


**Output**: governance.json


### Oracles


To reduce the number of IOracle contracts deployed we use MultiOracles that allow to access several data sources of the same type for a single contract.


```
npx hardhat run --network kovan core/13_oracles.ts
```



The oracles deployed are:


```
   chainlinkOracle: ChainlinkMultiOracle,
   compoundOracle: CompoundMultiOracle,
   compositeOracle: CompositeMultiOracle,
   cTokenOracle: CTokenMultiOracle,
```



The Timelock and the Cloak get ROOT permission on these contracts, while the deployer gets his permissions removed. The Timelock gets access to all the governance functions in these contracts.


**Input**: governance.json, protocol.json


**Output**: protocol.json


### Factories


We use the Wand to ensure that adding assets and series to the protocol is a robust process. To allow the Wand to deploy Join, FYToken and Pool contracts, and stay within the Spurious Dragon 24Kb contract size limit, we use factories.


```
npx hardhat run --network kovan core/13_factories.ts
```



The factories deployed are:


```
   poolFactory: PoolFactory,
   joinFactory: JoinFactory,
   fyTokenFactory: FYTokenFactory,
```



The Timelock and the Cloak get ROOT permission on these contracts, while the deployer gets his permissions removed. The Timelock gets access to all the governance functions in these contracts.


To deploy the factories, the libraries need to have been deployed first.


**Input**: governance.json, protocol.json


**Output**: protocol.json


### Cauldron


The Cauldron is the accounting contract for the Yield v2 Protocol.


```
npx hardhat run --network kovan core/14_cauldron.ts
```



The Timelock and the Cloak get ROOT permission on the Cauldron, while the deployer gets his permissions removed. The Timelock gets access to all the governance functions in the Cauldron.


**Input**: governance.json, protocol.json


**Output**: protocol.json


### Ladle


The Ladle is the routing contract for the Yield v2 Protocol.


```
npx hardhat run --network kovan core/15_ladle.ts
```



The Timelock and the Cloak get ROOT permission on the Ladle, while the deployer gets his permissions removed. The Timelock gets access to all the governance functions in the Ladle.


The Ladle gets permission to use several permissioned functions in the Cauldron. A plan is stored in the Cloak to pause or remove those permissions in an emergency.


To deploy the Ladle, the Cauldron needs to have been deployed first.


**Input**: governance.json, protocol.json


**Output**: protocol.json


### Witch	


The Witch is the liquidations contract for the Yield v2 Protocol.


```
npx hardhat run --network kovan core/16_Witch.ts
```



The Timelock and the Cloak get ROOT permission on the Witch, while the deployer gets his permissions removed. The Timelock gets access to all the governance functions in the Witch.


The Witch gets permission to use several permissioned functions in the Cauldron. A plan is stored in the Cloak to pause or remove those permissions in an emergency.


To deploy the Witch, the Ladle (and therefore Cauldron) needs to have been deployed first.


**Input**: governance.json, protocol.json


**Output**: protocol.json


### Wand


The Wand is a helper contract to add assets and series to the Yield v2 Protocol. While its use is not strictly necessary, it is recommended to avoid mistakes on these operations.


```
npx hardhat run --network kovan core/17_Wand.ts
```



The Timelock and the Cloak get ROOT permission on the Wand, while the deployer gets his permissions removed. The Timelock gets access to all the governance functions in the Wand.


The Wand gets permission to use several permissioned functions in the Cauldron, Ladle, Witch and Factories. A plan is stored in the Cloak to pause or remove those permissions in an emergency.


To deploy the Wand, the Cauldron, Ladle, Witch and Factories need to have been deployed first.


To execute the `makeBase` function, appropriate `chi` and `rate` data sources need to have been set in an oracle.


To execute the `makeIlk` function, appropriate `spot` data sources need to have been set in an oracle.


To execute the `addSeries` function, `makeBase` needs to have been executed first for the chosen underlying.


**Input**: governance.json, protocol.json


**Output**: protocol.json


## Operations


The operations are expected to be repeatedly executed during the lifetime of Yield v2. 


### Chi Sources


The Chi Sources produce data for fyToken redemption, and need to be present before an asset can be made into a base. Currently we only support Compound cTokens as a source of Chi.


```
npx hardhat run --network kovan operations/20_updateChiSource.ts
```



The script includes inside the bytes6 identifiers and addresses of the cTokens to use.


```
 const newSources: Array<[string, string]> = [
   [DAI,  "0xD9D6D61bc216a0BE1EecA4155b258CbB3030d23f"],
   [USDC, "0x078FfF3582342a16a7b038E6F4Fc9E88F738143d"],
   [USDT, "0x1292BAe0Ba398F9e480251b8C40D2A171EC446D1"],
   // [stringToBytes6('TST3'), "0x8A93d247134d91e0de6f96547cB0204e5BE8e5D8"],
 ]
```



The sources are set in the compoundOracle from protocol.json, as an assetId/CHI pair.


```
proposal.push({
  target: compoundOracle.address,
  data: compoundOracle.interface.encodeFunctionData("setSource", [baseId, CHI, sourceAddress])
})
```



The script reads the current chiSources.json to update it with the new sources. If there are no current sources an empty json Map must be provided in chiSources.json.


```
{"dataType":"Map","value":[]}
```



**Input**: governance.json, protocol.json, chiSources.json (for updating)


**Output**: chiSources.json


### Rate Sources


The Rate Sources produce data for debt accrual after maturity, and need to be present before an asset can be made into a base. Currently we only support Compound cTokens as a source of Rate.


```
npx hardhat run --network kovan operations/21_updateRateSource.ts
```



The script includes inside the bytes6 identifiers and addresses of the cTokens to use.


```
 const newSources: Array<[string, string]> = [
   [DAI,  "0x78F3579FbBb4a9894cE27cC201216Ef46A132f1c"],
   [USDC, "0x625E23f0C081CF8a2bbb87738087D8b0A5f5F412"],
   [USDT, "0xBce93F0D091092D780C6A386fb6d34780FFb031C"],
   // [stringToBytes6('TST3'), "0x8A93d247134d91e0de6f96547cB0204e5BE8e5D8"],
 ]
```



The sources are set in the compoundOracle from protocol.json, as an assetId/RATE pair.


```
proposal.push({
  target: compoundOracle.address,
  data: compoundOracle.interface.encodeFunctionData("setSource", [
    baseId,
    RATE,
    sourceAddress
  ])
})
```



The script reads the current rateSources.json to update it with the new sources. If there are no current sources an empty json Map must be provided in rateSources.json.


```
{"dataType":"Map","value":[]}
```



**Input**: governance.json, protocol.json, rateSources.json (for updating)


**Output**: rateSources.json


### Spot Sources


The Spot Sources produce data for vault collateralization checks, and need to be present before an asset can be made into a ilk. Currently we support Chainlink aggregators as a source of Spot.


When setting up a source, the reverse is also set. The ChainlinkMultiOracle will also always return the input amount as the output amount when a base is the same as the quote.


```
npx hardhat run --network kovan operations/22_updateSpotSource.ts
```



The script includes inside the bytes6 identifiers of base and quote, the string identifier for the appropriate MultiOracle in protocol.json, and addresses of the aggregators to use.


```
 const newSources: Array<[string, string, string, string]> = [
   [DAI, ETH,   'chainlinkOracle', "0x22B58f1EbEDfCA50feF632bD73368b2FdA96D541"],
   [USDC, ETH,  'chainlinkOracle', "0x64EaC61A2DFda2c3Fa04eED49AA33D021AeC8838"],
   [USDT, ETH,  'chainlinkOracle', "0x0bF499444525a23E7Bb61997539725cA2e928138"],
   [WBTC, ETH,  'chainlinkOracle', "0xF7904a295A029a3aBDFFB6F12755974a958C7C25"]
 ]
```



The sources are set in the specified IOracle from protocol.json, as an baseId/quoteId pair.


```
proposal.push({
  target: oracle.address,
  data: oracle.interface.encodeFunctionData("setSource", [
    baseId,
    quoteId,
    sourceAddress
  ])
})
```



The script reads the current spotSources.json to update it with the new sources. If there are no current sources an empty json Map must be provided in spotSources.json.


```
{"dataType":"Map","value":[]}
```



**Input**: governance.json, protocol.json, spotSources.json (for updating)


**Output**: spotSources.json


### CToken Sources


The cToken Sources produce spot data to use cToken as collateral, and need to be present before an cToken can be made into an ilk.


```
npx hardhat run --network kovan operations/23_updateCTokenSource.ts
```



The script includes inside the bytes6 identifiers of the cToken and its underlying, the string identifier for the CTokenMultiOracle, and addresses of the cTokens to use.


```
 const newSources: Array<[string, string, string, string]> = [
   [CDAI, DAI, 'cTokenOracle',   "0xf0d0eb522cfa50b716b3b1604c4f0fa6f04376ad"],
   [CUSDC, USDC, 'cTokenOracle', "0x4a92e71227d294f041bd82dd8f78591b75140d63"],
   [CUSDT, USDT, 'cTokenOracle', "0x3f0a0ea2f86bae6362cf9799b523ba06647da018"]
 ]
```



The sources are set in the compoundOracle from protocol.json, as an assetId/CHI pair.


```
proposal.push({
  target: cTokenOracle.address,
  data: cTokenOracle.interface.encodeFunctionData("setSource", [baseId, quoteId, sourceAddress])
})
```



The script reads the current cTokenSources.json to update it with the new sources. If there are no current sources an empty json Map must be provided in cTokenSources.json.


```
{"dataType":"Map","value":[]}
```



**Input**: governance.json, protocol.json, cTokenSources.json (for updating)


**Output**: cTokenSources.json


### Composite Pairs


When there isn’t an available source of Spot for a given token pair, we will combine several IOracle Spot feeds using a CompositeMultiOracle. First we must configure the CompositeMultiOracle with the available Spot sources. There is no current use for the CompositeMultiOracle.


```
npx hardhat run --network kovan operations/24_updateCompositePairs.ts
```



The script includes inside the bytes6 identifiers of base and quote, and the string identifier for the appropriate IOracle in protocol.json.


```
 const newCompositePairs: Array<[string, string, string]> = [
   /*[DAI, ETH, 'chainlinkOracle'],
   [USDC, ETH, 'chainlinkOracle'],
   [USDT, ETH, 'chainlinkOracle'],
   [WBTC, ETH, 'chainlinkOracle'],*/
 ]
```



The sources are set in the specified IOracle from protocol.json, as an baseId/quoteId pair.


```
proposal.push({
  target: compositeOracle.address,
  data: compositeOracle.interface.encodeFunctionData("setSource", [
    baseId,
    quoteId, 
    protocol.get(oracleName) as string
  ])
})
```



**Input**: governance.json, protocol.json


### Composite Paths


When there isn’t an available source of Spot for a given token pair, we will combine several IOracle Spot feeds using a CompositeMultiOracle. Now we must configure the CompositeMultiOracle with the appropriate paths between the Pairs that were added in the previous step. There is no current use for the CompositeMultiOracle.


```
npx hardhat run --network kovan operations/25_updateCompositePaths.ts
```



The script includes inside the bytes6 identifiers of base and quote, and an array with the bytes6 identifiers of the intermediate steps. For example `[DAI, USDC, [ETH]]` means that to calculate the DAI/USDC price we will combine the DAI/ETH and the ETH/USDC prices.


```
 const newCompositePaths: Array<[string, string, Array<string>]> = [
   /* [DAI, USDC, [ETH]],
   [DAI, USDT, [ETH]],
   [DAI, WBTC, [ETH]],
   [USDC, DAI, [ETH]],
   [USDC, USDT, [ETH]],
   [USDC, WBTC, [ETH]],
   [USDT, DAI, [ETH]],
   [USDT, USDC, [ETH]],
   [USDT, WBTC, [ETH]] */
 ]
```



The paths are set in the specified CompositeMultiOracle from protocol.json, enabling a baseId/quoteId pair.


```
proposal.push({
  target: compositeOracle.address,
  data: compositeOracle.interface.encodeFunctionData("setPath", [
    baseId,
    quoteId,
    path
  ])
})
```



**Input**: governance.json, protocol.json


### Add Assets


Adding assets makes them known to the Yield v2 core contracts using the Wand, and deploys a Join for them, but doesn’t set them up as collateral or underlying yet. This is a prerequisite to any other steps involving assets.


```
npx hardhat run --network kovan operations/30_addAsset.ts
```



The script includes inside the bytes6 identifiers and addresses of the assets to use.


```
 const newAssets: Array<[string, string]> = [
   [DAI,  "0xaFCdc724EB8781Ee721863db1B15939675996484"],
   [USDC, "0xeaCB3AAB4CA68F1e6f38D56bC5FCc499B76B4e2D"],
   [ETH,  "0x55C0458edF1D8E07DF9FB44B8960AecC515B4492"],
   [TST,  "0x51C9B30BE0417559A467D1f65d710a73E5845B3a"],
   [WBTC, "0xD5FafCE68897bdb55fA11Dd77858Df7a9a458D92"],
   [USDT, "0x233551369dc535f5fF3517c28fDCce81d650063e"],
   [CDAI,  "0xf0d0eb522cfa50b716b3b1604c4f0fa6f04376ad"],
   [CUSDC, "0x4a92e71227d294f041bd82dd8f78591b75140d63"],
   [CUSDT, "0x3f0a0ea2f86bae6362cf9799b523ba06647da018"],
   // [stringToBytes6('TST3'), "0xfaAddC93baf78e89DCf37bA67943E1bE8F37Bb8c"],
 ]
```



The script reads the current joins.json to update it with the new contracts. If there are no current Join contracts an empty json Map must be provided in joins.json.


```
{"dataType":"Map","value":[]}
```



As part of the Join deployment process, the Wand obtains ROOT permissions on the Join, and gives the same role to the deployer (in this case, the Timelock). It also gives `JOIN` and `EXIT` permissions to the Ladle.


The Timelock obtains permission to use the governance functions in the deployed Joins. A plan is stored in the Cloak to isolate the new Join from the Ladle in an emergency.


**Input**: governance.json, protocol.json, assets.json, joins.json (for updating)


**Output**: joins.json


### Make Bases


Making an asset into a base allows using it as an underlying for series and pools in Yield v2. This is done using the Wand, and is a prerequisite to use the asset as a base in a series.


```
npx hardhat run --network kovan operations/31_makeBase.ts
```



The script includes inside the bytes6 identifiers of the assets to make into bases, as well as the string identifier in protocol.json of the IOracle with the assetId/RATE pair.


```
 const newBases: Array<[string, string]> = [
   [DAI,  'compoundOracle'],
   [USDC, 'compoundOracle'],
   [USDT, 'compoundOracle'],
   // [stringToBytes6('TST3'), 'compoundOracle'],
 ]
```



The script verifies that the Chi and Rate sources for the assets supplied are present in the IOracle specified.


As part of this process, the Wand gives `JOIN` permissions to the Witch. A plan is stored in the Cloak to remove this permission in an emergency.


**Input**: governance.json, protocol.json


### Make Ilks


Making an asset into an ilk for a given base allows using it as an collateral for vaults of corresponding series in Yield v2. This is done using the Wand.


```
npx hardhat run --network kovan operations/32_makeIlk.ts
```



The script includes inside:


 - An existing base (bytes6 identifier)


 - The asset to make into an ilk for the base (bytes6 identifier)


 - The string identifier in protocol.json of the IOracle with the baseId/ilkId pair. This can currently be the chainlinkOracle or the compositeOracle.


 - The collateralization ratio, with 6 decimal places. 1000000 = 100%.


 - The debt ceiling for the base/ilk pair, with some zeros added afterwards.


 - The dust level for the base/ilk pair, with some zeros added afterwards.


 - The number of zeros to add to the debt ceiling and dust level. Note this needs to be the decimals of the base in the base/ilk pair.


```
 const newIlks: Array<[string, string, string, number, number, number, number]> = [
   // [DAI, stringToBytes6('TST1'), 'chainlinkOracle', 1000000, 1000000, 1, 18],
   [DAI, DAI, CHAINLINK, 1000000, 1000000, 1, 18], // Constant 1
   [DAI, USDC, CHAINLINK, 1000000, 1000000, 1, 18], // Via ETH
   [DAI, ETH, CHAINLINK, 1000000, 1000000, 1, 18],
   [DAI, WBTC, CHAINLINK, 1000000, 1000000, 1, 18], // Via ETH
   [DAI, USDT, CHAINLINK, 1000000, 1000000, 1, 18], // Via ETH
   [USDC, USDC, CHAINLINK, 1000000, 1000000, 1, 6], // Constant 1
   [USDC, DAI, CHAINLINK, 1000000, 1000000, 1, 6], // Via ETH
   [USDC, ETH, CHAINLINK, 1000000, 1000000, 1, 6],
   [USDC, WBTC, CHAINLINK, 1000000, 1000000, 1, 6], // Via ETH
   [USDC, USDT, CHAINLINK, 1000000, 1000000, 1, 6], // Via ETH
   [USDT, USDT, CHAINLINK, 1000000, 1000000, 1, 18], // Constant 1
   [USDT, DAI, CHAINLINK, 1000000, 1000000, 1, 18], // Via ETH
   [USDT, USDC, CHAINLINK, 1000000, 1000000, 1, 18], // Via ETH
   [USDT, ETH, CHAINLINK, 1000000, 1000000, 1, 18],
   [USDT, WBTC, CHAINLINK, 1000000, 1000000, 1, 18], // Via ETH
   [DAI, CDAI, CTOKEN, 1000000, 1000000, 1, 18],
   [USDC, CUSDC, CTOKEN, 1000000, 1000000, 1, 6],
   [USDT, CUSDT, CTOKEN, 1000000, 1000000, 1, 18],
 ]
```



The script verifies that the Spot sources for the pairs supplied are present in the IOracle specified.


As part of this process, the Wand gives `EXIT` permissions to the Witch.  A plan is stored in the Cloak to remove this permission in an emergency. 


**Input**: governance.json, protocol.json


### Add Series


Adding a series is a complex process that deploys a fyToken and a Pool contracts. This is done using the Wand.


```
npx hardhat run --network kovan operations/33_addSeries.ts
```



The script includes inside the bytes6 identifiers of:


 - A bytes6 identifier for the series, which gets LP added for the pool symbol.


 - The bytes6 identifier of the asset to use as an underlying.


 - The maturity of the series, in unix time.


 - An array of assets to be used as ilks for the series.


 - The name for the fyToken, which gets added “LP Token” for the corresponding pool.


 - The symbol for the fyToken, which gets added “LP” for the corresponding pool.


```
 const newSeries: Array<[string, string, number, string[], string, string]> = [
//    [stringToBytes6('DAI26'), DAI, EO2608, [TST1, TST2, TST3], 'DAI26', 'DAI26'],
//    [stringToBytes6('DAI27'), DAI, EO2708, [TST1, TST2, TST3], 'DAI27', 'DAI27'],
//    [stringToBytes6('DAI28'), DAI, EO2808, [TST1, TST2, TST3], 'DAI28', 'DAI28'],
   [stringToBytes6('DAI1'), DAI, EOSEP21, [DAI, USDC, ETH, TST, WBTC, USDT], 'DAI1', 'DAI1'], // Sep21
   [stringToBytes6('DAI2'), DAI, EODEC21, [DAI, USDC, ETH, TST, WBTC, USDT], 'DAI2', 'DAI2'], // Dec21
   [stringToBytes6('USDC1'), USDC, EOSEP21, [USDC, DAI, ETH, TST, WBTC, USDT], 'USDC1', 'USDC1'],
   [stringToBytes6('USDC2'), USDC, EODEC21, [USDC, DAI, ETH, TST, WBTC, USDT], 'USDC2', 'USDC2'],
   [stringToBytes6('USDT1'), USDT, EOSEP21, [USDT, DAI, USDC, ETH, TST, WBTC], 'USDT1', 'USDT1'],
   [stringToBytes6('USDT2'), USDT, EODEC21, [USDT, DAI, USDC, ETH, TST, WBTC], 'USDT2', 'USDT2']
 ]
```



The asset supplied as base needs to have been made into a base, and the ilks supplied need to have been made into ilks for the base supplied.


The deployment of a series costs about 10M gas, so only one can be done per block. The script uses the Timelock for deployment, but does one proposal per series instead of bundling them as for the rest of the protocol.


As part of the Join deployment process, the Timelock obtains ROOT permissions on the fyToken. The fyToken contract is also permissioned to `EXIT` from the Join for its underlying, so that it can be redeemed. The Ladle is permissioned to `MINT` and `BURN` fyToken.  A plan is stored in the Cloak to remove these permissions in an emergency.


The script reads the current fyTokens.json and pools.json to update it with the new contracts. If there are no current FYToken or Pool contracts an empty json Map must be provided in fyTokens.json and pools.json.


```
{"dataType":"Map","value":[]}
```



**Input**: governance.json, protocol.json, fyTokens.json (for updating), pools.json (for updating)


**Output**: fyTokens.json, pools.json


## Pools


### Pool Initialization


The initialization of pools can be done by bundling a transaction to transfer underlying to the pool, with a transaction to mint LP tokens. For testnet purposes a script to initialize all pools and skew them to a 5% rate is supplied. The Timelock is used as a transaction relayer.


```
npx hardhat run --network kovan operations/40_initPools.ts
```



**Input**: governance.json, pools.json


## Strategies


### Deployment


The deployment of liquidity strategies is done with a script, in the same manner as the core protocols were deployed.


```
npx hardhat run --network kovan core/19_strategies.ts
```



The input data for the script is located in config.ts. It includes the name and symbol for the strategy, as well as the bytes6 identifier for the base asset that will be common to all pools that the strategy works with.


```
export const strategiesData: Array<[string, string, string]> = [ // name, symbol, baseId
   ['DAI3M', 'DAI3M', DAI],
   ['DAI6M', 'DAI6M', DAI],
   ['USDC3M', 'USDC3M', USDC],
   ['USDC6M', 'USDC6M', USDC],
   ['USDT3M', 'USDT3M', USDT],
   ['USDT6M', 'USDT6M', USDT]
]
```



As part of the deployment process, the Timelock is given ROOT permissions to each Strategy, as well as permission to execute all governance actions.


**Input**: governance.json, protocol.json


**Output**: strategies.json


### Initialization


The initialization of strategies involves at least four steps, and is provided to be used in testnets.


```
npx hardhat run --network kovan operations/41_initStrategies.ts
```



The input data includes a string to be used as both name and symbol for the strategy, as well as the bytes6 identifiers for the first series/pool pair, and the next series pool pair. The bytes6 identifiers for a series and the corresponding pool usually match.


```
const strategiesInit: Array<[string, [string, string], [string, string]]> = [
   ['DAI2S', 
[stringToBytes6('DAI21'), stringToBytes6('DAI21')],
[stringToBytes6('DAI22'), stringToBytes6('DAI22')]],
   ['USDC2S', 
[stringToBytes6('USDC21'), stringToBytes6('USDC21')],
[stringToBytes6('USDC22'), stringToBytes6('USDC22')]],
   ['USDT2S', 
[stringToBytes6('USDT21'), stringToBytes6('USDT21')],
[stringToBytes6('USDT22'), stringToBytes6('USDT22')]],
   ['USDC3D', 
[stringToBytes6('USDC26'), stringToBytes6('USDC26')],
[stringToBytes6('USDC27'), stringToBytes6('USDC27')]],
 ]
```



The strategy needs to be supplied with underlying to mint the initial Strategy tokens. We mint the underlying in testnets, but in mainnet it would be done with a transferFrom.


```
proposal.push(
  {
    target: base.address,
    data: base.interface.encodeFunctionData("mint", [
      strategy.address,
      baseUnit.mul(1000)
    ])
  },
)
```



As part of the initialization process, the Strategy is added as an Integration to the Ladle, to allow the Ladle to route calls to the Strategy.


```
proposal.push(
  {
    target: ladle.address,
    data: ladle.interface.encodeFunctionData("addIntegration", [
      strategy.address,
      true
    ])
  },
)
```



As part of the initialization process, the Strategy is added as a Token to the Ladle, to allow the Ladle to transfer and permit Strategy tokens.


```
proposal.push(
  {
    target: ladle.address,
    data: ladle.interface.encodeFunctionData("addToken", [
      strategy.address,
      true
    ])
  },
)
```



**Input**: governance.json, protocol.json, pools.json, strategies.json
