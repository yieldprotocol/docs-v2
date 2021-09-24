# Governance

## Actors

**Governor**: The governor is an account with complete control over the protocol, through a timelock. It is expected to be an x-of-n multisig, or a governance contract. Over time, there can be a hierarchy of governors, with different powers and parameters.

**Operations team**: The operations team has limited control to execute what is determined by governance. The AccessControl contract is implemented as a 1-of-n multisig and so individual members can directly be given permissions consistent with an operations role. In the future, an additional x-of-n multisig could be considered for the operations role. The operations role can also be subdivided into several roles, with different permissions and parameters.

**Timelock**: The Timelock channels all changes to the protocol. As such, it can execute any permissioned function in the protocol. No one else can execute any permissioned function in the protocol, except the Timelock. The exception to this are the permissioned functions in the Timelock, that can be called by the governor or operations team as explained above. As the protocol grows, there can be multiple Timelocks with different permissions and delays.

## Process

All changes to the protocol are executed through the Timelock.

1. The operations team will submit the proposal, which is a series of function calls to be executed from the Timelock, calling `propose` in the Timelock.
2. The owners of the multisig, or the community in its case, can independently decode the proposal to verify its contents. If it is collectively decided to approve the proposal, the governor account will call `approve` on the Timelock for the proposal hash.
3. The operations team will `execute` the approved proposal, no earlier than the delay set in the Timelock. The proposal will be executed from the Timelock.