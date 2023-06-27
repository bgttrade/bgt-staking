# BGT staking

### Staking Operations

#### delegate

Delegate BGT to validator.

- validator: Which validator you want to delegete.

msg.value: Send some BGT.

#### undelegate

Undelegate BGT from validator. You must have enough BGT on this validator to undelegate.

- address: Which validator you want to undelegate.
- amount: how many BGT you want to undelegate.

#### updateValidator

Update `memo`, `rate`. You must be a `staker` of the validator.

- validator: Which validator you want to update.
- memo: Same as stake.
- rate: Same as stake.

### Read Operations

#### State Variable

These `state variable` can access directly. Default value maybe change, don't hardcode it.

- stakeMininum: Mininum staking value; Default: 10000 BGT
- delegateMininum: Mininum delegate value; Default 1 unit (0.000001 BGT)
- powerRateMaximum: Maxinum rate of power, Default is 0.2 (200000)
- blocktime: Blocktime, Default is 16.
- unboundBlock: Lock time for undelegate.
- totalDelegationAmount; totol amount.
- maxDelegationAmountBasedOnTotalAmount(); Stake / Delegate value must less than this value.

#### Validators

- validators: mapping(address => Validator); Mapping address to validator:
- allValidatorsLength() -> usize; Get all validators count.
- allValidatorsAt(uint256) -> address; Get validator's address based on idx, the range is `0 .. allValidatorsLength()`.
- allValidatorsContains(address) -> bool; If an address is validator, return true;

Validator struct contain these field:

```solidity
struct Validator {
    // Public key of validator
    bytes public_key;
    // Public key type
    PublicKeyType ty;
    // memo of validator
    string memo;
    // rate of validator, decimal is 6
    uint256 rate;
    // address of staker
    address staker;
    // totol power of this validator
    uint256 power;
}
```

#### Delegators

- delegators: mapping(address => Delegator); Get Delegator info based on address

Delegators:

- delegatorsBoundAmount(address delegator, address validator); Delegate / Stake amount on validator. address is
  validator address.
- delegatorsUnboundAmount(address delegator, address validator); Total locked amount in undelegation.
- amount; Total amount for all validators.

Iterate all delegator:

- allDelegatorsLength(); Get all delegators count.
- allDelegatorsAt(uint256 idx) -> address; Get validator's address based on idx, the range is 0 ..
  allDelegatorsLength().
- allDelegatorsContains(address value) -> bool; If an address is delegator, return true;

Iterate `validator` to `delegator` . Get delegators under a validator:

- validatorOfDelegatorLength(address validator); Get all delegators count under validator.
- validatorOfDelegatorAt(address validator, uint256 idx) -> address; Get delegator address under validator.
- validatorOfDelegatorContains(address validator, address delegator) -> bool; If a validator have this delegator, return
  true.

Iterate `delegator` to `validator` . Get `delegator` stake to `validator`.

- delegatorOfValidatorLength(address delegator); Get all validators count is staked / delegated.
- delegatorOfValidatorAt(address delegator, uint256 idx) -> address; Get validator under delegator.
- delegatorOfValidatorContains(address delegator, address value) -> bool; If a delegator stake / delegate to validator,
  return true.

## Reward Operations

### claim

Use `claim(uint256 amount)` to claim data.

### Read reward amount

Use `rewards(address delegator)` to get rewards.

## Record

Record for delegate, undelegate and stake.

### Iterate by delegator

- Get length of record by `delegatorRecordIndexLength(address delegator) -> uint256`;
- Iterate by index `delegatorRecordIndex(address delegator, uint256 at) -> bytes32`;
- Get info of record by `records(bytes32 idx)`;

