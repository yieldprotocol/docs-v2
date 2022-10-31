# Solidity API

## NotionalJoin

### FlashFeeFactorSet

```solidity
event FlashFeeFactorSet(uint256 fee)
```

### Redeemed

```solidity
event Redeemed(uint256 fCash, uint256 underlying, uint256 accrual)
```

### FLASH_LOAN_RETURN

```solidity
bytes32 FLASH_LOAN_RETURN
```

### FLASH_LOANS_DISABLED

```solidity
uint256 FLASH_LOANS_DISABLED
```

### asset

```solidity
address asset
```

_asset managed by this contract_

### underlying

```solidity
address underlying
```

### maturity

```solidity
uint40 maturity
```

### currencyId

```solidity
uint16 currencyId
```

### id

```solidity
uint256 id
```

### storedBalance

```solidity
uint256 storedBalance
```

### accrual

```solidity
uint256 accrual
```

### flashFeeFactor

```solidity
uint256 flashFeeFactor
```

### constructor

```solidity
constructor(address asset_, address underlying_, uint40 maturity_, uint16 currencyId_) public
```

### afterMaturity

```solidity
modifier afterMaturity()
```

### beforeMaturity

```solidity
modifier beforeMaturity()
```

### supportsInterface

```solidity
function supportsInterface(bytes4 interfaceID) external view returns (bool)
```

_Advertising through ERC165 the available functions_

### onERC1155Received

```solidity
function onERC1155Received(address, address, uint256 _id, uint256, bytes) external returns (bytes4)
```

_Called by the sender after a transfer to verify it was received. Ensures only `id` tokens are received._

### onERC1155BatchReceived

```solidity
function onERC1155BatchReceived(address, address, uint256[] _ids, uint256[], bytes) external returns (bytes4)
```

_Called by the sender after a batch transfer to verify it was received. Ensures only `id` tokens are received._

### join

```solidity
function join(address user, uint128 amount) external returns (uint128)
```

_Take `amount` `asset` from `user` using `transferFrom`, minus any unaccounted `asset` in this contract._

### _join

```solidity
function _join(address user, uint128 amount) internal returns (uint128)
```

_Take `amount` `asset` from `user` using `transferFrom`, minus any unaccounted `asset` in this contract._

### exit

```solidity
function exit(address user, uint128 amount) external returns (uint128)
```

_Before maturity, transfer `amount` `asset` to `user`.
After maturity, withdraw if necessary, then transfer `amount.wmul(accrual)` `underlying` to `user`._

### _exit

```solidity
function _exit(address user, uint128 amount) internal returns (uint128)
```

_Transfer `amount` `asset` to `user`_

### _exitUnderlying

```solidity
function _exitUnderlying(address user, uint128 amount) internal returns (uint128)
```

_Transfer `amount` `underlying` to `user`_

### redeem

```solidity
function redeem() public
```

_Switch to an exit-only underlying Join, converting all fCash holdings to underlying in the process._

### retrieve

```solidity
function retrieve(contract IERC20 token, address to) external
```

_Retrieve any ERC20 tokens. Useful for airdropped tokens._

### retrieveERC1155

```solidity
function retrieveERC1155(contract ERC1155 token, uint256 id_, address to) external
```

_Retrieve any ERC1155 tokens other than the `asset`. Useful for airdropped tokens._

