# FYToken

### Point

```solidity
event Point(bytes32 param, address value)
```

### FlashFeeFactorSet

```solidity
event FlashFeeFactorSet(uint256 fee)
```

### SeriesMatured

```solidity
event SeriesMatured(uint256 chiAtMaturity)
```

### Redeemed

```solidity
event Redeemed(address from, address to, uint256 amount, uint256 redeemed)
```

### CHI_NOT_SET

```solidity
uint256 CHI_NOT_SET
```

### MAX_TIME_TO_MATURITY

```solidity
uint256 MAX_TIME_TO_MATURITY
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

### oracle

```solidity
contract IOracle oracle
```

### join

```solidity
contract IJoin join
```

_Source of redemption funds._

### underlying

```solidity
address underlying
```

_Asset that is returned on redemption._

### underlyingId

```solidity
bytes6 underlyingId
```

### maturity

```solidity
uint256 maturity
```

_Unix time at which redemption of fyToken for underlying are possible_

### chiAtMaturity

```solidity
uint256 chiAtMaturity
```

### constructor

```solidity
constructor(bytes6 underlyingId_, contract IOracle oracle_, contract IJoin join_, uint256 maturity_, string name, string symbol) public
```

### afterMaturity

```solidity
modifier afterMaturity()
```

### beforeMaturity

```solidity
modifier beforeMaturity()
```

### point

```solidity
function point(bytes32 param, address value) external
```

_Point to a different Oracle or Join_

### setFlashFeeFactor

```solidity
function setFlashFeeFactor(uint256 flashFeeFactor_) external
```

_Set the flash loan fee factor_

### mature

```solidity
function mature() external
```

_Mature the fyToken by recording the chi.
If called more than once, it will revert._

### _mature

```solidity
function _mature() private returns (uint256 _chiAtMaturity)
```

_Mature the fyToken by recording the chi._

### accrual

```solidity
function accrual() external returns (uint256)
```

_Retrieve the chi accrual since maturity, maturing if necessary._

### _accrual

```solidity
function _accrual() private returns (uint256 accrual_)
```

_Retrieve the chi accrual since maturity, maturing if necessary.
Note: Call only after checking we are past maturity_

### redeem

```solidity
function redeem(address to, uint256 amount) external returns (uint256 redeemed)
```

_Burn fyToken after maturity for an amount that increases according to `chi`
If `amount` is 0, the contract will redeem instead the fyToken balance of this contract. Useful for batches._

### mintWithUnderlying

```solidity
function mintWithUnderlying(address to, uint256 amount) external
```

_Mint fyToken providing an equal amount of underlying to the protocol_

### mint

```solidity
function mint(address to, uint256 amount) external
```

_Mint fyTokens._

### burn

```solidity
function burn(address from, uint256 amount) external
```

_Burn fyTokens. The user needs to have either transferred the tokens to this contract, or have approved this contract to take them._

### _burn

```solidity
function _burn(address from, uint256 amount) internal returns (bool)
```

_Burn fyTokens.
Any tokens locked in this contract will be burned first and subtracted from the amount to burn from the user's wallet.
This feature allows someone to transfer fyToken to this contract to enable a `burn`, potentially saving the cost of `approve` or `permit`._

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

_From ERC-3156. Loan `amount` fyDai to `receiver`, which needs to return them plus fee to this contract within the same transaction.
Note that if the initiator and the borrower are the same address, no approval is needed for this contract to take the principal + fee from the borrower.
If the borrower transfers the principal + fee to this contract, they will be burnt here instead of pulled from the borrower._

| Name | Type | Description |
| ---- | ---- | ----------- |
| receiver | contract IERC3156FlashBorrower | The contract receiving the tokens, needs to implement the `onFlashLoan(address user, uint256 amount, uint256 fee, bytes calldata)` interface. |
| token | address | The loan currency. Must be a fyDai contract. |
| amount | uint256 | The amount of tokens lent. |
| data | bytes | A data parameter to be passed on to the `receiver` for any custom use. |

