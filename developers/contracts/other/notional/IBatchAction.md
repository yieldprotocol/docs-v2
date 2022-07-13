# Solidity API

## IBatchAction

### DepositActionType

```solidity
enum DepositActionType {
  None,
  DepositAsset,
  DepositUnderlying,
  DepositAssetAndMintNToken,
  DepositUnderlyingAndMintNToken,
  RedeemNToken,
  ConvertCashToNToken
}
```

### BalanceAction

```solidity
struct BalanceAction {
  enum IBatchAction.DepositActionType actionType;
  uint16 currencyId;
  uint256 depositActionAmount;
  uint256 withdrawAmountInternalPrecision;
  bool withdrawEntireCashBalance;
  bool redeemToUnderlying;
}
```

### batchBalanceAction

```solidity
function batchBalanceAction(address account, struct IBatchAction.BalanceAction[] actions) external
```

Executes a batch of balance transfers including minting and redeeming nTokens.

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | the account for the action |
| actions | struct IBatchAction.BalanceAction[] | array of balance actions to take, must be sorted by currency id |

