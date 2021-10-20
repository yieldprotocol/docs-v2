# Emergencies


In Yield v2 there are two types of emergency procedures to effect change in the smart contracts as a response to an emergency: Emergency Plans, and Emergency Proposals.


**Emergency Plans**


Emergency plans are stored in the Cloak (EmergencyBrake) and allow removing the orchestration between contracts easily and safely. Executed emergency plans can be reverted easily by governance.


**Emergency Proposals**


For those predictable emergencies that cannot be resolved with an Emergency Plan, Emergency Proposals can be proposed and approved in the Timelock. The operations team can execute them rather easily in an emergency, and governance proposals can be passed, approved and executed to revert them. While more flexible, this approach is more complex and error-prone.


## Timelock


The Timelock has complete control over the protocol, and is likely to be the sole holder of the ROOT role in all contracts.


A compromised Timelock is likely to be catastrophic. If possible, the procedure to deploy a new Timelock and decommission the compromised one should be followed.


To add a new Timelock, it needs to be given permissions on the smart contracts that will fall under its control. The permissions given any or all of the governance functions in each of the contracts, and possibly the ROOT as well. Giving permissions is done through a proposal in an existing Timelock with ROOT permissions over the smart contracts in scope.


To remove an existing Timelock, the operations dashboard needs to be used to find its permissions in each contract in the protocol, and then a proposal passed through a Timelock with ROOT permissions over the contracts in scope. Special care is needed in not remaining without ROOT access to any smart contract, unless specifically intending to do so.


If there are no accounts with ROOT access to a smart contract, permissions cannot be changed for that smart contract ever again. The EmergencyBrake doesn’t count as having ROOT access in this context, as it can’t give new access control permissions.


## Protocol Pause


A complete pause of the protocol can be achieved by executing all Emergency Plans, preferably in this order:



1. All Joins, to protect assets.
2. Ladle and Witch, to protect accounting.
3. All FYToken, to protect fyToken supply.

This pause can be lifted easily by the governor through a special zero-delay Timelock.


If it is suspected that the Ladle is being abused to take assets out of user wallets, all token integrations should be removed from the Ladle using an Emergency Proposal. Note this includes assets, fyTokens, pools and strategies. This can only be restored through governance.


```
ladle.addToken(token, false)
```



## Cloak (EmergencyBrake)


The Cloak can temporarily disable the orchestration between smart contracts in the protocol. To do so it holds the ROOT role to all smart contracts under its scope.


In the case of a compromised Cloak, a proposal must be passed through the Timelock to revoke the Cloak ROOT permissions to all contracts under its scope. This will render the compromised Cloak powerless and effectively terminated.


## Oracle Sources


A malfunctioning oracle source can be disabled passing a proposal to set it to the zero address in the appropriate Oracle. Any further transactions requesting data for the related base and quote will revert.


**CompoundMultiOracle:**


```
oracle.setSource(baseId, quoteId, ZERO_ADDRESS)
```



**ChainlinkMultiOracle:**


```
oracle.setSource(baseId, ZERO_ADDRESS, quoteId, ZERO_ADDRESS, ZERO_ADDRESS)
```



## Oracles


A generalized Oracle malfunction, as opposed to a single source malfunction, can be addressed by removing the affected Oracle from the Cauldron or FYToken contracts.


**SpotOracle:**


A Timelock proposal is required to set the spot oracle to the zero address for **all** impacted base and ilk pairs. Any further transactions requiring collateralization checks will revert.


```
cauldron.setSpotOracle(baseId, ilkId, ZERO_ADDRESS, ratio)
```



**LendingOracle:**


A Timelock proposal is required to set in the Cauldron the lending oracle to the zero address for all impacted assets, the same proposal should also set the chi oracle to the zero address for all impacted FYToken. Any further transactions involving collateralization checks, FYToken maturing or redemptions will revert.


```
cauldron.setLendingOracle(baseId, ZERO_ADDRESS)
fyToken.point(bytes32('oracle'), ZERO_ADDRESS)
```



## Factories


A malfunctioning Factory (JoinFactory, FYTokenFactory, PoolFactory) is unlikely to become a time critical emergency, unless the Wand is compromised as well. Factories are called by the Wand to deploy and permission new assets, make them ilks and/or bases, and deploy and permission new series.


In the case of a compromised factory, the safest option is to execute the Wand Emergency plan. Any further transactions using the Wand will revert.


## Joins


A malfunctioning Join can compromise protocol assets, and action must be taken immediately. In such an emergency, execute a Protocol Pause.


## Cauldron


A malfunctioning Cauldron will be catastrophic, as it can’t be replaced. In such an emergency, execute a Protocol Pause. With the protocol effectively paused, a plan can be made to extract the assets from the Joins and return them to users according to a shutdown plan.


## Ladle


A malfunctioning Ladle is a very serious issue, and action must be taken immediately. In such an emergency, execute the Ladle Emergency Plan. Any transactions involving collateral or debt will revert after this, but liquidations, redemptions, and transactions involving only YieldSpace pools, Ladle modules, Ladle integrations or Ladle token transfers can still take place.


If required to disable Ladle modules, integrations or token transfers, read below.


### Tokens 


If necessary to remove a token integration, it must be done through an Emergency Proposal.


```
ladle.addToken(token, false)
```



### Integrations 


If necessary to remove a contract integration, it must be done through an Emergency Proposal.


```
ladle.addIntegration(integration, false)
```



### Modules


If necessary to remove a module integration, it must be done through an Emergency Proposal.


```
ladle.addModule(module, false)
```



## Witch


A malfunctioning Witch can be a very serious issue, affecting all protocol solvency, accountancy, and user assets. In such an emergency, execute the Emergency Plan for the Witch, which will disconnect it from the Cauldron, and all the Emergency Plans disconnecting the Witch from a Join. There will be many of the latter.


## Wand


The Wand is not user-facing, but a compromised Wand could be used to add compromised assets to the protocol, which in turn could be used for further attacks. In such an emergency, execute the Emergency Plan for the Wand, which will effectively disable it.


## Pools


The Pools themselves are not controlled by governance, but in the case of a compromised Pool it can be removed from the Ladle using an Emergency Proposal to remove the related integrations. Note that the pools are usually deployed using the PoolFactory, in which case all Pools deployed by the same factory should be removed.


```
ladle.addToken(pool, false)
ladle.addIntegration(pool, false)
```



## Strategies


In a case of a Strategy malfunction, it can be removed from the Ladle. However, this will not be completely effective in protecting user assets, as the Strategy functions can be executed directly.


```
ladle.addToken(strategy, false)
ladle.addIntegration(strategy, false)
