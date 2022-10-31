# Solidity API

## FCashMock

### underlying

```solidity
contract ERC20Mock underlying
```

### fCashId

```solidity
uint256 fCashId
```

### accrual

```solidity
uint256 accrual
```

### constructor

```solidity
constructor(contract ERC20Mock underlying_, uint256 fCashId_) public
```

### uri

```solidity
function uri(uint256) public view virtual returns (string)
```

### setAccrual

```solidity
function setAccrual(uint256 accrual_) external
```

### mint

```solidity
function mint(address to, uint256 id, uint256 amount, bytes data) external
```

### batchBalanceAction

```solidity
function batchBalanceAction(address account, struct IBatchAction.BalanceAction[] actions) external
```

