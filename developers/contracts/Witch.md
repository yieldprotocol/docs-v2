# Solidity API

## Witch

### Point

```solidity
event Point(bytes32 param, address value)
```

### IlkSet

```solidity
event IlkSet(bytes6 ilkId, uint32 duration, uint64 initialOffer, uint96 line, uint24 dust, uint8 dec)
```

### Bought

```solidity
event Bought(bytes12 vaultId, address buyer, uint256 ink, uint256 art)
```

### Auctioned

```solidity
event Auctioned(bytes12 vaultId, uint256 start)
```

### Auction

```solidity
struct Auction {
  address owner;
  uint32 start;
}
```

### Ilk

```solidity
struct Ilk {
  uint32 duration;
  uint64 initialOffer;
}
```

### Limits

```solidity
struct Limits {
  uint96 line;
  uint24 dust;
  uint8 dec;
  uint128 sum;
}
```

### cauldron

```solidity
contract ICauldron cauldron
```

### ladle

```solidity
contract ILadle ladle
```

### auctions

```solidity
mapping(bytes12 => struct Witch.Auction) auctions
```

### ilks

```solidity
mapping(bytes6 => struct Witch.Ilk) ilks
```

### limits

```solidity
mapping(bytes6 => struct Witch.Limits) limits
```

### constructor

```solidity
constructor(contract ICauldron cauldron_, contract ILadle ladle_) public
```

### point

```solidity
function point(bytes32 param, address value) external
```

_Point to a different ladle_

### setIlk

```solidity
function setIlk(bytes6 ilkId, uint32 duration, uint64 initialOffer, uint96 line, uint24 dust, uint8 dec) external
```

_Governance function to set:
 - the auction duration to calculate liquidation prices
 - the proportion of the collateral that will be sold at auction start
 - the maximum collateral that can be auctioned at the same time
 - the minimum collateral that must be left when buying, unless buying all
 - The decimals for maximum and minimum_

### auction

```solidity
function auction(bytes12 vaultId) external
```

_Put an undercollateralized vault up for liquidation._

### buy

```solidity
function buy(bytes12 vaultId, uint128 base, uint128 min) external returns (uint256 ink)
```

_Pay `base` of the debt in a vault in liquidation, getting at least `min` collateral.
Use `payAll` to pay all the debt, using `buy` for amounts close to the whole vault might revert._

### payAll

```solidity
function payAll(bytes12 vaultId, uint128 min) external returns (uint256 ink)
```

_Pay all debt from a vault in liquidation, getting at least `min` collateral._

### settle

```solidity
function settle(address user, bytes6 ilkId, bytes6 baseId, uint128 ink, uint128 art) private
```

_Move base from the buyer to the protocol, and collateral from the protocol to the buyer_

### inkPrice

```solidity
function inkPrice(struct DataTypes.Balances balances, uint256 initialOffer_, uint256 duration_, uint256 elapsed) private pure returns (uint256 price)
```

_Price of a collateral unit, in underlying, at the present moment, for a given vault. Rounds up, sometimes twice.
           ink                     min(auction, elapsed)
price = (------- * (p + (1 - p) * -----------------------))
           art                          auction_

### _isVaultUndercollateralised

```solidity
function _isVaultUndercollateralised(bytes12 vaultId) internal virtual returns (bool)
```

