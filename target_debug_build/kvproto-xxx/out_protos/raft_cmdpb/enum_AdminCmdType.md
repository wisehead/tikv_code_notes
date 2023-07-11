#1.enum AdminCmdType

```rust
pub enum AdminCmdType {
    InvalidAdmin = 0,
    ChangePeer = 1,
    Split = 2,
    CompactLog = 3,
    TransferLeader = 4,
    ComputeHash = 5,
    VerifyHash = 6,
    PrepareMerge = 7,
    CommitMerge = 8,
    RollbackMerge = 9,
    BatchSplit = 10,
    ChangePeerV2 = 11,
    PrepareFlashback = 12,
    FinishFlashback = 13,
    BatchSwitchWitness = 14,
    UpdateGcPeer = 15,
}
```