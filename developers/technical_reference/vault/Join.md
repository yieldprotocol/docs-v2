


## Functions
### constructor
```solidity
  function constructor(
  ) public
```




### setFlashFeeFactor
```solidity
  function setFlashFeeFactor(
  ) external
```

Set the flash loan fee factor


### join
```solidity
  function join(
  ) external returns (uint128)
```

Take `amount` `asset` from `user` using `transferFrom`, minus any unaccounted `asset` in this contract.


### _join
```solidity
  function _join(
  ) internal returns (uint128)
```

Take `amount` `asset` from `user` using `transferFrom`, minus any unaccounted `asset` in this contract.


### exit
```solidity
  function exit(
  ) external returns (uint128)
```

Transfer `amount` `asset` to `user`


### _exit
```solidity
  function _exit(
  ) internal returns (uint128)
```

Transfer `amount` `asset` to `user`


### retrieve
```solidity
  function retrieve(
  ) external
```

Retrieve any tokens other than the `asset`. Useful for airdropped tokens.


### maxFlashLoan
```solidity
  function maxFlashLoan(
    address token
  ) external returns (uint256)
```

From ERC-3156. The amount of currency available to be lended.

#### Parameters:
| Name | Type | Description                                                          |
| :--- | :--- | :------------------------------------------------------------------- |
|`token` | address | The loan currency. It must be a FYDai contract.

#### Return Values:
| Name                           | Type          | Description                                                                  |
| :----------------------------- | :------------ | :--------------------------------------------------------------------------- |
|`The`| address | amount of `token` that can be borrowed.
### flashFee
```solidity
  function flashFee(
    address token,
    uint256 amount
  ) external returns (uint256)
```

From ERC-3156. The fee to be charged for a given loan.

#### Parameters:
| Name | Type | Description                                                          |
| :--- | :--- | :------------------------------------------------------------------- |
|`token` | address | The loan currency. It must be the asset.
|`amount` | uint256 | The amount of tokens lent.

#### Return Values:
| Name                           | Type          | Description                                                                  |
| :----------------------------- | :------------ | :--------------------------------------------------------------------------- |
|`The`| address | amount of `token` to be charged for the loan, on top of the returned principal.
### _flashFee
```solidity
  function _flashFee(
    uint256 amount
  ) internal returns (uint256)
```

The fee to be charged for a given loan.

#### Parameters:
| Name | Type | Description                                                          |
| :--- | :--- | :------------------------------------------------------------------- |
|`amount` | uint256 | The amount of tokens lent.

#### Return Values:
| Name                           | Type          | Description                                                                  |
| :----------------------------- | :------------ | :--------------------------------------------------------------------------- |
|`The`| uint256 | amount of `token` to be charged for the loan, on top of the returned principal.
### flashLoan
```solidity
  function flashLoan(
    contract IERC3156FlashBorrower receiver,
    address token,
    uint256 amount,
    bytes data
  ) external returns (bool)
```

From ERC-3156. Loan `amount` `asset` to `receiver`, which needs to return them plus fee to this contract within the same transaction.
If the principal + fee are transferred to this contract, they won't be pulled from the receiver.

#### Parameters:
| Name | Type | Description                                                          |
| :--- | :--- | :------------------------------------------------------------------- |
|`receiver` | contract IERC3156FlashBorrower | The contract receiving the tokens, needs to implement the `onFlashLoan(address user, uint256 amount, uint256 fee, bytes calldata)` interface.
|`token` | address | The loan currency. Must be a fyDai contract.
|`amount` | uint256 | The amount of tokens lent.
|`data` | bytes | A data parameter to be passed on to the `receiver` for any custom use.

## Events
### FlashFeeFactorSet
```solidity
  event FlashFeeFactorSet(
  )
```



