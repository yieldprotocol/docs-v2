# Solidity API

## CTokenInterface

### borrowIndex

```solidity
function borrowIndex() external view returns (uint256)
```

Accumulator of the total earned interest rate since the opening of the market

### exchangeRateCurrent

```solidity
function exchangeRateCurrent() external returns (uint256)
```

Accrue interest then return the up-to-date exchange rate

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Calculated exchange rate scaled by 1e18 |

### exchangeRateStored

```solidity
function exchangeRateStored() external view returns (uint256)
```

Calculates the exchange rate from the underlying to the CToken

_This function does not accrue interest before calculating the exchange rate_

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Calculated exchange rate scaled by 1e18 |

### underlying

```solidity
function underlying() external view returns (address)
```

Underlying asset for this CToken

