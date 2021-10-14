# Governance


The Yield v2 contracts have individual access control features for each permissioned function. At its very minimum, two actors will be needed to operate the protocol safely: The **governor** and the **operations team**. The permissions to the protocol of these two actors are channeled through the **Timelock** and the **EmergencyBrake**.


## Actors


**Governor**: The governor is an account with complete control over the protocol, through a timelock. It is expected to be an x-of-n multisig, or a governance contract.


**Operations team**: The operations team has limited control to execute what is determined by governance. The AccessControl contract is implemented as a 1-of-n multisig and so individual members can directly be given permissions consistent with an operations role. In the future, an additional x-of-n multisig could be considered for the operations role.


This is a minimal setup suitable for launch. Over time, as the protocol grows, it is expected that the governor and operations roles will be subdivided and possibly placed in a hierarchy, to reduce the risk profile of any individual account.


## Timelock


The **Timelock** has complete control over the protocol. It has the `ROOT` permission to all contracts, which allows the Timelock to `grant` and `revoke` permissions in all contracts. It also has permission to execute all the governance functions in all contracts. The Timelock can grant itself permission to execute non-governance functions in the contracts as well, even if that is not expected to be a common occurrence.


In the Timelock, the **governor** has permissions to `propose`, `approve` and `execute`. In that sense the governor has complete control over the protocol, subject to the `delay` set in the Timelock, which is the only safety mechanism that protects the protocol after a governance takeover. It’s important to note that a governance takeover could well be fatal for the protocol and must be avoided at all costs.


The governor is not expected to `propose` or `execute` proposals, and has those permissions as a protection against a coordinated attack by the operations team. The governor has no other permissions over the protocol apart from controlling the Timelock.


In the Timelock, the **operations team** has permissions to `propose` and `execute`. The operations team can set up the proposals, and once they are approved by the governor, can execute them at an appropriate time.


## EmergencyBrake


The **EmergencyBrake** has a partial control over the granting and revoking permissions over the entire protocol. It has the `ROOT` permission to all contracts, but its implementation only allows the EmergencyBrake to grant and revoke permissions in a predetermined manner. The EmergencyBrake doesn’t have access to any permissioned function in any contract, it can’t grant permission to itself to any contract, and it is not expected to be granted any permission either.


In the EmergencyBrake, the **Timelock** has the permission to `plan` emergencies, which will usually be set up in the same proposal that orchestrates new contracts. The Timelock also has permission to `restore` or `terminate` orchestrations that have been paused by the execution of a plan.


In the EmergencyBrake, the **operations team** has the permission to `execute` plans, effectively pausing parts of the protocol.
