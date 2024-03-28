# ISablierV2LockupTranched

[Git Source](https://github.com/sablier-labs/v2-core/blob/e080f20eafef0fc18049bcc77f1694db043860f1/src/interfaces/ISablierV2LockupTranched.sol)

**Inherits:** [ISablierV2Lockup](/docs/contracts/v2/reference/core/interfaces/interface.ISablierV2Lockup.md)

Creates and manages Lockup streams with tranches.

## Functions

### getRange

Retrieves the stream's range, which is a struct documented in {DataTypes}.

_Reverts if `streamId` references a null stream._

```solidity
function getRange(uint256 streamId) external view returns (LockupTranched.Range memory range);
```

**Parameters**

| Name       | Type      | Description                  |
| ---------- | --------- | ---------------------------- |
| `streamId` | `uint256` | The stream id for the query. |

### getStream

Retrieves the stream details, which is a struct documented in {DataTypes}.

_Reverts if `streamId` references a null stream._

```solidity
function getStream(uint256 streamId) external view returns (LockupTranched.StreamLT memory stream);
```

**Parameters**

| Name       | Type      | Description                  |
| ---------- | --------- | ---------------------------- |
| `streamId` | `uint256` | The stream id for the query. |

### getTranches

Retrieves the tranches the protocol uses to compose the custom distribution curve.

_Reverts if `streamId` references a null stream._

```solidity
function getTranches(uint256 streamId) external view returns (LockupTranched.Tranche[] memory tranches);
```

**Parameters**

| Name       | Type      | Description                  |
| ---------- | --------- | ---------------------------- |
| `streamId` | `uint256` | The stream id for the query. |

### MAX_TRANCHE_COUNT

The maximum number of tranches allowed in a stream.

_This is initialized at construction time and cannot be changed later._

```solidity
function MAX_TRANCHE_COUNT() external view returns (uint256);
```

### createWithDurations

Creates a stream by setting the start time to `block.timestamp`, and the end time to the sum of `block.timestamp` and
all specified time durations. The tranche timestamps are derived from these durations. The stream is funded by
`msg.sender` and is wrapped in an ERC-721 NFT.

Emits a {Transfer} and {CreateLockupTrancheStream} event. Requirements:

- All requirements in {createWithTimestamps} must be met for the calculated parameters.

```solidity
function createWithDurations(LockupTranched.CreateWithDurations calldata params) external returns (uint256 streamId);
```

**Parameters**

| Name     | Type                                 | Description                                                                        |
| -------- | ------------------------------------ | ---------------------------------------------------------------------------------- |
| `params` | `LockupTranched.CreateWithDurations` | Struct encapsulating the function parameters, which are documented in {DataTypes}. |

**Returns**

| Name       | Type      | Description                         |
| ---------- | --------- | ----------------------------------- |
| `streamId` | `uint256` | The id of the newly created stream. |

### createWithTimestamps

Creates a stream with the provided tranche timestamps, implying the end time from the last timestamp. The stream is
funded by `msg.sender` and is wrapped in an ERC-721 NFT.

Emits a {Transfer} and {CreateLockupTrancheStream} event. Notes:

- As long as the tranche timestamps are arranged in ascending order, it is not an error for some of them to be in the
  past. Requirements:
- Must not be delegate called.
- `params.totalAmount` must be greater than zero.
- If set, `params.broker.fee` must not be greater than `MAX_BROKER_FEE`.
- `params.startTime` must be greater than zero and less than the first tranche's timestamp
- `params.tranches` must have at least one tranche, but not more than `MAX_TRANCHE_COUNT`.
- The tranche timestamps must be arranged in ascending order.
- The last tranche timestamp (i.e. the stream's end time) must be in the future.
- The sum of the tranche amounts must equal the deposit amount.
- `params.recipient` must not be the zero address.
- `msg.sender` must have allowed this contract to spend at least `params.totalAmount` assets.

```solidity
function createWithTimestamps(LockupTranched.CreateWithTimestamps calldata params)
    external
    returns (uint256 streamId);
```

**Parameters**

| Name     | Type                                  | Description                                                                        |
| -------- | ------------------------------------- | ---------------------------------------------------------------------------------- |
| `params` | `LockupTranched.CreateWithTimestamps` | Struct encapsulating the function parameters, which are documented in {DataTypes}. |

**Returns**

| Name       | Type      | Description                         |
| ---------- | --------- | ----------------------------------- |
| `streamId` | `uint256` | The id of the newly created stream. |

## Events

### CreateLockupTranchedStream

Emitted when a stream is created.

```solidity
event CreateLockupTranchedStream(
    uint256 streamId,
    address funder,
    address indexed sender,
    address indexed recipient,
    Lockup.CreateAmounts amounts,
    IERC20 indexed asset,
    bool cancelable,
    bool transferable,
    LockupTranched.Tranche[] tranches,
    LockupTranched.Range range,
    address broker
);
```

**Parameters**

| Name           | Type                       | Description                                                                                                              |
| -------------- | -------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `streamId`     | `uint256`                  | The id of the newly created stream.                                                                                      |
| `funder`       | `address`                  | The address which has funded the stream.                                                                                 |
| `sender`       | `address`                  | The address from which to stream the assets, who will have the ability to cancel the stream.                             |
| `recipient`    | `address`                  | The address toward which to stream the assets.                                                                           |
| `amounts`      | `Lockup.CreateAmounts`     | Struct containing (i) the deposit amount, and (ii) the broker fee amount, both denoted in units of the asset's decimals. |
| `asset`        | `IERC20`                   | The contract address of the ERC-20 asset used for streaming.                                                             |
| `cancelable`   | `bool`                     | Boolean indicating whether the stream will be cancelable or not.                                                         |
| `transferable` | `bool`                     | Boolean indicating whether the stream NFT is transferable or not.                                                        |
| `tranches`     | `LockupTranched.Tranche[]` | The tranches the protocol uses to compose the custom streaming curve.                                                    |
| `range`        | `LockupTranched.Range`     | Struct containing (i) the stream's start time and (ii) end time, both as Unix timestamps.                                |
| `broker`       | `address`                  | The address of the broker who has helped create the stream, e.g. a front-end website.                                    |