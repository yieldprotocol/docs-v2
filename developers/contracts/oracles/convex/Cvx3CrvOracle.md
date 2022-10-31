# Solidity API

## Cvx3CrvOracle

Provides current values for Cvx3Crv

_Both peek() (view) and get() (transactional) are provided for convenience_

### threecrv

```solidity
contract ICurvePool threecrv
```

### DAI

```solidity
contract AggregatorV3Interface DAI
```

### USDC

```solidity
contract AggregatorV3Interface USDC
```

### USDT

```solidity
contract AggregatorV3Interface USDT
```

### cvx3CrvId

```solidity
bytes32 cvx3CrvId
```

### ethId

```solidity
bytes32 ethId
```

### SourceSet

```solidity
event SourceSet(bytes32 cvx3CrvId_, bytes32 ethId_, contract ICurvePool threecrv_, contract AggregatorV3Interface DAI_, contract AggregatorV3Interface USDC_, contract AggregatorV3Interface USDT_)
```

### setSource

```solidity
function setSource(bytes32 cvx3CrvId_, bytes32 ethId_, contract ICurvePool threecrv_, contract AggregatorV3Interface DAI_, contract AggregatorV3Interface USDC_, contract AggregatorV3Interface USDT_) external
```

Set threecrv pool and the chainlink sources

| Name | Type | Description |
| ---- | ---- | ----------- |
| cvx3CrvId_ | bytes32 | cvx3crv Id |
| ethId_ | bytes32 | ETH ID |
| threecrv_ | contract ICurvePool | The 3CRV pool address |
| DAI_ | contract AggregatorV3Interface | DAI/ETH chainlink price feed address |
| USDC_ | contract AggregatorV3Interface | USDC/ETH chainlink price feed address |
| USDT_ | contract AggregatorV3Interface | USDT/ETH chainlink price feed address |

### min

```solidity
function min(uint256 a, uint256 b) internal pure returns (uint256)
```

_Returns the smallest of two numbers._

### peek

```solidity
function peek(bytes32 base, bytes32 quote, uint256 baseAmount) external view virtual returns (uint256 quoteAmount, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price.

_Only cvx3crvid and ethId are accepted as asset identifiers._

| Name | Type | Description |
| ---- | ---- | ----------- |
| base | bytes32 | Id of base token |
| quote | bytes32 | Id of quoted token |
| baseAmount | uint256 | Amount of base token for which to get a quote |

| Name | Type | Description |
| ---- | ---- | ----------- |
| quoteAmount | uint256 | Total amount in terms of quoted token |
| updateTime | uint256 | Time quote was last updated |

### get

```solidity
function get(bytes32 base, bytes32 quote, uint256 baseAmount) external virtual returns (uint256 quoteAmount, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price. Same as `peek` for this oracle.

_Only cvx3crvid and ethId are accepted as asset identifiers._

| Name | Type | Description |
| ---- | ---- | ----------- |
| base | bytes32 | Id of base token |
| quote | bytes32 | Id of quoted token |
| baseAmount | uint256 | Amount of base token for which to get a quote |

| Name | Type | Description |
| ---- | ---- | ----------- |
| quoteAmount | uint256 | Total amount in terms of quoted token |
| updateTime | uint256 | Time quote was last updated |

### _peek

```solidity
function _peek(bytes6 base, bytes6 quote, uint256 baseAmount) private view returns (uint256 quoteAmount, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price.

_Only cvx3crvid and ethId are accepted as asset identifiers._

| Name | Type | Description |
| ---- | ---- | ----------- |
| base | bytes6 | Id of base token |
| quote | bytes6 | Id of quoted token |
| baseAmount | uint256 | Amount of base token for which to get a quote |

| Name | Type | Description |
| ---- | ---- | ----------- |
| quoteAmount | uint256 | Total amount in terms of quoted token |
| updateTime | uint256 | Time quote was last updated |

