# Solidity API

## IWstETH

### getWstETHByStETH

```solidity
function getWstETHByStETH(uint256 _stETHAmount) external view returns (uint256)
```

Get amount of wstETH obtained for a given amount of stETH

| Name | Type | Description |
| ---- | ---- | ----------- |
| _stETHAmount | uint256 | amount of stETH |

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Amount of wstETH obtained for a given stETH amount |

### getStETHByWstETH

```solidity
function getStETHByWstETH(uint256 _wstETHAmount) external view returns (uint256)
```

Get amount of stETH obtained for a given amount of wstETH

| Name | Type | Description |
| ---- | ---- | ----------- |
| _wstETHAmount | uint256 | amount of wstETH |

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Amount of stETH obtained for a given wstETH amount |

### stEthPerToken

```solidity
function stEthPerToken() external view returns (uint256)
```

Get amount of stETH obtained for one wstETH

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Amount of stETH obtained for one wstETH |

### tokensPerStEth

```solidity
function tokensPerStEth() external view returns (uint256)
```

Get amount of wstETH obtained for one stETH

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Amount of wstETH obtained for one stETH |

### wrap

```solidity
function wrap(uint256 _stETHAmount) external returns (uint256)
```

Exchanges stETH to wstETH

_Requirements:
 - `_stETHAmount` must be non-zero
 - msg.sender must approve at least `_stETHAmount` stETH to this
   contract.
 - msg.sender must have at least `_stETHAmount` of stETH.
User should first approve _stETHAmount to the WstETH contract_

| Name | Type | Description |
| ---- | ---- | ----------- |
| _stETHAmount | uint256 | amount of stETH to wrap in exchange for wstETH |

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Amount of wstETH user receives after wrap |

### unwrap

```solidity
function unwrap(uint256 _wstETHAmount) external returns (uint256)
```

Exchanges wstETH to stETH

_Requirements:
 - `_wstETHAmount` must be non-zero
 - msg.sender must have at least `_wstETHAmount` wstETH._

| Name | Type | Description |
| ---- | ---- | ----------- |
| _wstETHAmount | uint256 | amount of wstETH to uwrap in exchange for stETH |

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | Amount of stETH user receives after unwrap |

