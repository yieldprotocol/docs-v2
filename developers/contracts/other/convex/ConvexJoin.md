# Solidity API

## ConvexJoin

Wrapper used to manage staking of Convex tokens

### EarnedData

```solidity
struct EarnedData {
  address token;
  uint256 amount;
}
```

### RewardType

```solidity
struct RewardType {
  address reward_token;
  address reward_pool;
  uint128 reward_integral;
  uint128 reward_remaining;
  mapping(address &#x3D;&gt; uint256) reward_integral_for;
  mapping(address &#x3D;&gt; uint256) claimable_reward;
}
```

### managed_assets

```solidity
uint256 managed_assets
```

### vaults

```solidity
mapping(address => bytes12[]) vaults
```

### crv

```solidity
address crv
```

### cvx

```solidity
address cvx
```

### convexToken

```solidity
address convexToken
```

### convexPool

```solidity
address convexPool
```

### convexPoolId

```solidity
uint256 convexPoolId
```

### cauldron

```solidity
contract ICauldron cauldron
```

### rewards

```solidity
struct ConvexJoin.RewardType[] rewards
```

### registeredRewards

```solidity
mapping(address => uint256) registeredRewards
```

### CRV_INDEX

```solidity
uint256 CRV_INDEX
```

### CVX_INDEX

```solidity
uint256 CVX_INDEX
```

### _status

```solidity
uint8 _status
```

### _NOT_ENTERED

```solidity
uint8 _NOT_ENTERED
```

### _ENTERED

```solidity
uint8 _ENTERED
```

### Deposited

```solidity
event Deposited(address _user, address _account, uint256 _amount, bool _wrapped)
```

### Withdrawn

```solidity
event Withdrawn(address _user, uint256 _amount, bool _unwrapped)
```

### VaultAdded

```solidity
event VaultAdded(address account, bytes12 vaultId)
```

Event called when a vault is added for a user

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | The account for which vault is added |
| vaultId | bytes12 | The vaultId to be added |

### VaultRemoved

```solidity
event VaultRemoved(address account, bytes12 vaultId)
```

Event called when a vault is removed for a user

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | The account for which vault is removed |
| vaultId | bytes12 | The vaultId to be removed |

### constructor

```solidity
constructor(address _convexToken, address _convexPool, uint256 _poolId, contract ICauldron _cauldron, address _crv, address _cvx) public
```

### nonReentrant

```solidity
modifier nonReentrant()
```

### setApprovals

```solidity
function setApprovals() public
```

Give maximum approval to the pool & convex booster contract to transfer funds from wrapper

### addVault

```solidity
function addVault(bytes12 vaultId) external
```

Adds a vault to the user's vault list

| Name | Type | Description |
| ---- | ---- | ----------- |
| vaultId | bytes12 | The id of the vault being added |

### removeVault

```solidity
function removeVault(bytes12 vaultId, address account) public
```

Remove a vault from the user's vault list

| Name | Type | Description |
| ---- | ---- | ----------- |
| vaultId | bytes12 | The id of the vault being removed |
| account | address | The user from whom the vault needs to be removed |

### aggregatedAssetsOf

```solidity
function aggregatedAssetsOf(address account) internal view returns (uint256)
```

Get user's balance of collateral deposited in various vaults

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | User's address for which balance is requested |

| Name | Type | Description |
| ---- | ---- | ----------- |
| [0] | uint256 | User's balance of collateral |

### addRewards

```solidity
function addRewards() public
```

Adds reward tokens by reading the available rewards from the RewardStaking pool

_CRV token is added as a reward by default_

### join

```solidity
function join(address user, uint128 amount) external returns (uint128)
```

Take convex LP token and credit it to the `user` address.

_Before the join is called the vault is already updated, so the balance needs to be adjusted to the previous state for calculating the checkpoint_

### exit

```solidity
function exit(address user, uint128 amount) external returns (uint128)
```

Debit convex LP tokens held by this contract and send them to the `user` address.

_IMPORTANT: Checkpoint needs to be called before calling pour for exit
since the vault is updated before calling exit calling checkpoint here would result in an incorrect calculation_

### _calcRewardIntegral

```solidity
function _calcRewardIntegral(uint256 index, address account, uint256 balance, bool claim) internal
```

Calculates & upgrades the integral for distributing the reward token

| Name | Type | Description |
| ---- | ---- | ----------- |
| index | uint256 | The index of the reward token for which the calculations are to be done |
| account | address | Account for which the CvxIntegral has to be calculated |
| balance | uint256 | Balance of the accounts |
| claim | bool | Whether to claim the calculated rewards |

### _checkpoint

```solidity
function _checkpoint(address account, uint256 delta, bool claim) internal
```

Create a checkpoint for the supplied addresses by updating the reward integrals & claimable reward for them & claims the rewards

_Before the join is called the vault is already updated, so the balance needs to be adjusted to the previous state for calculating the checkpoint_

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | The account for which checkpoints have to be calculated |
| delta | uint256 | Amount to be subtracted from depositedBalance while joining |
| claim | bool | Whether to claim the rewards for the account |

### checkpoint

```solidity
function checkpoint(address account) external returns (bool)
```

Create a checkpoint for the supplied addresses by updating the reward integrals & claimable reward for them

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | The accounts for which checkpoints have to be calculated |

### getReward

```solidity
function getReward(address account) external
```

Claim reward for the supplied account

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | Address whose reward is to be claimed |

### earned

```solidity
function earned(address account) external view returns (struct ConvexJoin.EarnedData[] claimable)
```

Get the amount of tokens the user has earned

| Name | Type | Description |
| ---- | ---- | ----------- |
| account | address | Address whose balance is to be checked |

| Name | Type | Description |
| ---- | ---- | ----------- |
| claimable | struct ConvexJoin.EarnedData[] | Array of earned tokens and their amount |

