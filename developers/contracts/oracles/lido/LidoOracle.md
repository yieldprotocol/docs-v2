# Solidity API

## LidoOracle

Oracle to fetch WstETH-stETH exchange amounts

### wstETH

```solidity
contract IWstETH wstETH
```

### wstEthId

```solidity
bytes32 wstEthId
```

### stEthId

```solidity
bytes32 stEthId
```

### SourceSet

```solidity
event SourceSet(contract IWstETH wstETH)
```

### constructor

```solidity
constructor(bytes32 wstEthId_, bytes32 stEthId_) public
```

### setSource

```solidity
function setSource(contract IWstETH wstETH_) external
```

Set the source for fetching the price from. It should be the wstETH contract.

### peek

```solidity
function peek(bytes32 base, bytes32 quote, uint256 baseAmount) external view virtual returns (uint256 quoteAmount, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price.
Only `wstEthId` and `stEthId` are accepted as asset identifiers.

### get

```solidity
function get(bytes32 base, bytes32 quote, uint256 baseAmount) external virtual returns (uint256 quoteAmount, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price. Same as `peek` for this oracle.
Only `wstEthId` and `stEthId` are accepted as asset identifiers.

### _peek

```solidity
function _peek(bytes6 base, bytes6 quote, uint256 baseAmount) private view returns (uint256 quoteAmount, uint256 updateTime)
```

Retrieve the value of the amount at the latest oracle price.
Only `wstEthId` and `stEthId` are accepted as asset identifiers.

