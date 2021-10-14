
Router forwards calls between two contracts, so that any permissions
given to the original caller are stripped from the call.
This is useful when implementing generic call routing functions on contracts
that might have ERC20 approvals or AccessControl authorizations.

## Functions
### route
```solidity
  function route(
  ) external returns (bytes result)
```

Allow users to route calls to a pool, to be used with batch


