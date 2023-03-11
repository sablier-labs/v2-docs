# LockupPro
[Git Source](https://github.com/sablierhq/v2-core/blob/8b6a851f4185bd5af0e89a0f6a6eb2fed069cd10/docs/contracts/v2/reference/core/abstracts)

Quasi-namespace for the structs used in the {SablierV2LockupPro} contract.


## Structs
### CreateWithDeltas
Struct that encapsulates the parameters of the {SablierV2LockupPro-createWithDeltas} function.


```solidity
struct CreateWithDeltas {
    address sender;
    address recipient;
    uint128 totalAmount;
    IERC20 asset;
    bool cancelable;
    LockupPro.SegmentWithDelta[] segments;
    Broker broker;
}
```

### CreateWithMilestones
Struct that encapsulates the parameters of the {SablierV2LockupPro-createWithMilestones} function.


```solidity
struct CreateWithMilestones {
    LockupPro.Segment[] segments;
    address sender;
    uint40 startTime;
    bool cancelable;
    address recipient;
    uint128 totalAmount;
    IERC20 asset;
    Broker broker;
}
```

### Range
Range struct used as a field in the lockup pro stream.


```solidity
struct Range {
    uint40 start;
    uint40 end;
}
```

### Segment
Segment struct used in the {SablierV2LockupPro} contract.


```solidity
struct Segment {
    uint128 amount;
    UD2x18 exponent;
    uint40 milestone;
}
```

### SegmentWithDelta
Segment struct used in the {SablierV2LockupPro-createWithDeltas} function.


```solidity
struct SegmentWithDelta {
    uint128 amount;
    UD2x18 exponent;
    uint40 delta;
}
```

### Stream
Pro stream struct used in the {SablierV2LockupPro} contract.

*The fields are arranged like this to save gas via tight variable packing.*


```solidity
struct Stream {
    Lockup.Amounts amounts;
    Segment[] segments;
    address sender;
    uint40 startTime;
    uint40 endTime;
    bool isCancelable;
    Lockup.Status status;
    IERC20 asset;
}
```
