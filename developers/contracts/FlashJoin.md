# FlashJoin

### FlashFeeFactorSet

```solidity
event FlashFeeFactorSet(uint256 fee)
```

### FLASH_LOAN_RETURN

```solidity
bytes32 FLASH_LOAN_RETURN
```

### FLASH_LOANS_DISABLED

```solidity
uint256 FLASH_LOANS_DISABLED
```

### flashFeeFactor

```solidity
uint256 flashFeeFactor
```

### constructor

```solidity
constructor(address asset_) public
```

### setFlashFeeFactor

```solidity
function setFlashFeeFactor(uint256 flashFeeFactor_) external
```

_Set the flash loan fee factor_

### maxFlashLoan

```solidity
function maxFlashLoan(address token) external view returns (uint256)
```

_From ERC-3156. The amount of currency available to be lended._

| Name | Type | Description |
| ---- | ---- | ----------- |
| token | address | The loan currency. It must be a FYDai contract. |

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | The amount of `token` that can be borrowed. |

### flashFee

```solidity
function flashFee(address token, uint256 amount) external view returns (uint256)
```

_From ERC-3156. The fee to be charged for a given loan._

| Name | Type | Description |
| ---- | ---- | ----------- |
| token | address | The loan currency. It must be the asset. |
| amount | uint256 | The amount of tokens lent. |

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | The amount of `token` to be charged for the loan, on top of the returned principal. |

### _flashFee

```solidity
function _flashFee(uint256 amount) internal view returns (uint256)
```

_The fee to be charged for a given loan._

| Name | Type | Description |
| ---- | ---- | ----------- |
| amount | uint256 | The amount of tokens lent. |

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | The amount of `token` to be charged for the loan, on top of the returned principal. |

### flashLoan

```solidity
function flashLoan(contract IERC3156FlashBorrower receiver, address token, uint256 amount, bytes data) external returns (bool)
```

_From ERC-3156. Loan `amount` `asset` to `receiver`, which needs to return them plus fee to this contract within the same transaction.
If the principal + fee are transferred to this contract, they won't be pulled from the receiver._

| Name | Type | Description |
| ---- | ---- | ----------- |
| receiver | contract IERC3156FlashBorrower | The contract receiving the tokens, needs to implement the `onFlashLoan(address user, uint256 amount, uint256 fee, bytes calldata)` interface. |
| token | address | The loan currency. Must be a fyDai contract. |
| amount | uint256 | The amount of tokens lent. |
| data | bytes | A data parameter to be passed on to the `receiver` for any custom use. |

