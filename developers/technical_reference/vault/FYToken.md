


## Functions
### constructor
```solidity
  function constructor(
  ) public
```




### point
```solidity
  function point(
  ) external
```

Point to a different Oracle or Join


### mature
```solidity
  function mature(
  ) external
```

Mature the fyToken by recording the chi.
If called more than once, it will revert.


### accrual
```solidity
  function accrual(
  ) external returns (uint256)
```

Retrieve the chi accrual since maturity, maturing if necessary.


### redeem
```solidity
  function redeem(
  ) external returns (uint256 redeemed)
```

Burn fyToken after maturity for an amount that increases according to `chi`
If `amount` is 0, the contract will redeem instead the fyToken balance of this contract. Useful for batches.


### mintWithUnderlying
```solidity
  function mintWithUnderlying(
  ) external
```

Mint fyToken providing an equal amount of underlying to the protocol


### mint
```solidity
  function mint(
  ) external
```

Mint fyTokens.


### burn
```solidity
  function burn(
  ) external
```

Burn fyTokens. The user needs to have either transferred the tokens to this contract, or have approved this contract to take them.


### _burn
```solidity
  function _burn(
  ) internal returns (bool)
```

Burn fyTokens. 
Any tokens locked in this contract will be burned first and subtracted from the amount to burn from the user's wallet.
This feature allows someone to transfer fyToken to this contract to enable a `burn`, potentially saving the cost of `approve` or `permit`.


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
    address token
  ) external returns (uint256)
```

From ERC-3156. The fee to be charged for a given loan.

#### Parameters:
| Name | Type | Description                                                          |
| :--- | :--- | :------------------------------------------------------------------- |
|`token` | address | The loan currency. It must be a FYDai.
param amount The amount of tokens lent.

#### Return Values:
| Name                           | Type          | Description                                                                  |
| :----------------------------- | :------------ | :--------------------------------------------------------------------------- |
|`The`| address | amount of `token` to be charged for the loan, on top of the returned principal.
### flashLoan
```solidity
  function flashLoan(
    contract IERC3156FlashBorrower receiver,
    address token,
    uint256 amount,
    bytes data
  ) external returns (bool)
```

From ERC-3156. Loan `amount` fyDai to `receiver`, which needs to return them plus fee to this contract within the same transaction.
Note that if the initiator and the borrower are the same address, no approval is needed for this contract to take the principal + fee from the borrower.
If the borrower transfers the principal + fee to this contract, they will be burnt here instead of pulled from the borrower.

#### Parameters:
| Name | Type | Description                                                          |
| :--- | :--- | :------------------------------------------------------------------- |
|`receiver` | contract IERC3156FlashBorrower | The contract receiving the tokens, needs to implement the `onFlashLoan(address user, uint256 amount, uint256 fee, bytes calldata)` interface.
|`token` | address | The loan currency. Must be a fyDai contract.
|`amount` | uint256 | The amount of tokens lent.
|`data` | bytes | A data parameter to be passed on to the `receiver` for any custom use.

## Events
### Point
```solidity
  event Point(
  )
```



### SeriesMatured
```solidity
  event SeriesMatured(
  )
```



### Redeemed
```solidity
  event Redeemed(
  )
```



