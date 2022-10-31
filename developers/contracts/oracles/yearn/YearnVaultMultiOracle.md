# Solidity API

## YearnVaultMultiOracle

Provides current values for Yearn Vault tokens (e.g. yvUSDC/USDC)

_Both peek() and get() are provided for convenience
       Prices are calculated, never based on cached values_

### SourceSet

```solidity
event SourceSet(bytes6 baseId, bytes6 quoteId, address source, uint8 decimals)
```

### Source

```solidity
struct Source {
  address source;
  uint8 decimals;
  bool inverse;
}
```

### sources

```solidity
mapping(bytes6 => mapping(bytes6 => struct YearnVaultMultiOracle.Source)) sources
```

This is a registry of baseId => quoteId => Source
       used to look up the Yearn vault address needed to calculate share price

### setSource

```solidity
function setSource(bytes6 underlyingId, bytes6 vaultTokenId, contract IYvToken vaultToken) external
```

Set or reset a Yearn Vault Token oracle source and its inverse

_parameter ORDER IS crucial!  If id's are out of order the math will be wrong_

| Name | Type | Description |
| ---- | ---- | ----------- |
| underlyingId | bytes6 | id used for underlying base token (e.g. USDC) |
| vaultTokenId | bytes6 | address for Yearn vault token |
| vaultToken | contract IYvToken | address for Yearn vault token |

### _setSource

```solidity
function _setSource(bytes6 baseId, bytes6 quoteId, contract IYvToken source, uint8 decimals, bool inverse) internal
```

internal function to set source and emit event

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseId | bytes6 | id used for base token |
| quoteId | bytes6 | id for quote (represents vaultToken when inverse == false) |
| source | contract IYvToken | address for vault token used to determine price |
| decimals | uint8 | used by vault token (both source and base) |
| inverse | bool | set true for inverse pairs (e.g. USDC/yvUSDC) |

### get

```solidity
function get(bytes32 baseId, bytes32 quoteId, uint256 amountBase) external returns (uint256 amountQuote, uint256 updateTime)
```

External function to convert amountBase base at the current vault share price

_This external function calls _peek() which calculates current (not cached) price_

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseId | bytes32 | id of base (denominator of rate used) |
| quoteId | bytes32 | id of quote (returned amount in this) |
| amountBase | uint256 | amount in base to convert to amount in quote |

| Name | Type | Description |
| ---- | ---- | ----------- |
| amountQuote | uint256 | product of exchange rate and amountBase |
| updateTime | uint256 | current block timestamp |

### peek

```solidity
function peek(bytes32 baseId, bytes32 quoteId, uint256 amountBase) external view returns (uint256 amountQuote, uint256 updateTime)
```

External function to convert amountBase at the current vault share price

_This function is exactly the same as get() and provided as a convenience
       for contracts that need to call peek_

### _peek

```solidity
function _peek(bytes6 baseId, bytes6 quoteId, uint256 amountBase) internal view returns (uint256 amountQuote, uint256 updateTime)
```

Used to convert a given amount using the current vault share price

_This internal function is called by external functions peek() and get()_

| Name | Type | Description |
| ---- | ---- | ----------- |
| baseId | bytes6 | id of base (denominator of rate used) |
| quoteId | bytes6 | id of quote (returned amount converted to this) |
| amountBase | uint256 | amount in base to convert to amount in quote |

| Name | Type | Description |
| ---- | ---- | ----------- |
| amountQuote | uint256 | product of exchange rate and amountBase |
| updateTime | uint256 | current block timestamp |

