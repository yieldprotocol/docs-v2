# Solidity API

## CvxMining

Contains function to calc amount of CVX to mint from a given amount of CRV

### cvx

```solidity
contract ICvx cvx
```

### ConvertCrvToCvx

```solidity
function ConvertCrvToCvx(uint256 _amount) internal view returns (uint256)
```

| Name | Type | Description |
| ---- | ---- | ----------- |
| _amount | uint256 | The amount of CRV to burn |

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | The amount of CVX to mint |

