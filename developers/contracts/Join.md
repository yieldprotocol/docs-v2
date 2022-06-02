# Solidity API

## Join

### asset

```solidity
address asset
```

_asset managed by this contract_

### storedBalance

```solidity
uint256 storedBalance
```

### constructor

```solidity
constructor(address asset_) public
```

### join

```solidity
function join(address user, uint128 amount) external virtual returns (uint128)
```

_Take `amount` `asset` from `user` using `transferFrom`, minus any unaccounted `asset` in this contract._

### _join

```solidity
function _join(address user, uint128 amount) internal returns (uint128)
```

_Take `amount` `asset` from `user` using `transferFrom`, minus any unaccounted `asset` in this contract._

### exit

```solidity
function exit(address user, uint128 amount) external virtual returns (uint128)
```

_Transfer `amount` `asset` to `user`_

### _exit

```solidity
function _exit(address user, uint128 amount) internal returns (uint128)
```

_Transfer `amount` `asset` to `user`_

### retrieve

```solidity
function retrieve(contract IERC20 token, address to) external
```

_Retrieve any tokens other than the `asset`. Useful for airdropped tokens._

