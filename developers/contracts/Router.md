# Router

_Router forwards calls between two contracts, so that any permissions
given to the original caller are stripped from the call.
This is useful when implementing generic call routing functions on contracts
that might have ERC20 approvals or AccessControl authorizations._

### owner

```solidity
address owner
```

### constructor

```solidity
constructor() public
```

### route

```solidity
function route(address target, bytes data) external payable returns (bytes result)
```

_Allow users to route calls to a pool, to be used with batch_

