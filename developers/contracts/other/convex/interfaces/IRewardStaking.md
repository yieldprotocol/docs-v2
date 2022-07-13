# Solidity API

## IRewardStaking

### stakeFor

```solidity
function stakeFor(address, uint256) external
```

### stake

```solidity
function stake(uint256) external
```

### withdraw

```solidity
function withdraw(uint256 amount, bool claim) external
```

### withdrawAndUnwrap

```solidity
function withdrawAndUnwrap(uint256 amount, bool claim) external
```

### earned

```solidity
function earned(address account) external view returns (uint256)
```

### getReward

```solidity
function getReward() external
```

### getReward

```solidity
function getReward(address _account, bool _claimExtras) external
```

### extraRewardsLength

```solidity
function extraRewardsLength() external view returns (uint256)
```

### extraRewards

```solidity
function extraRewards(uint256 _pid) external view returns (address)
```

### rewardToken

```solidity
function rewardToken() external view returns (address)
```

### balanceOf

```solidity
function balanceOf(address _account) external view returns (uint256)
```

