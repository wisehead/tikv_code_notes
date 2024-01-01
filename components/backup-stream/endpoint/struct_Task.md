#1.struct Task

```rust
pub enum Task {
    WatchTask(TaskOp),
    BatchEvent(Vec<CmdBatch>),
    ChangeConfig(BackupStreamConfig),
    /// Change the observe status of some region.
    ModifyObserve(ObserveOp),
    /// Convert status of some task into `flushing` and do flush then.
    ForceFlush(String),
    /// FatalError pauses the task and set the error.
    FatalError(TaskSelector, Box<Error>),
    /// Run the callback when see this message. Only for test usage.
    /// NOTE: Those messages for testing are not guarded by `#[cfg(test)]` for
    /// now, because the integration test would not enable test config when
    /// compiling (why?)
    Sync(
        // Run the closure if ...
        Box<dyn FnOnce() + Send>,
        // This returns `true`.
        // The argument should be `self`, but there are too many generic argument for `self`...
        // So let the caller in test cases downcast this to the type they need manually...
        Box<dyn FnMut(&mut dyn Any) -> bool + Send>,
    ),
    /// Mark the store as a failover store.
    /// This would prevent store from updating its checkpoint ts for a while.
    /// Because we are not sure whether the regions in the store have new leader
    /// -- we keep a safe checkpoint so they can choose a safe `from_ts` for
    /// initial scanning.
    MarkFailover(Instant),
    /// Flush the task with name.
    Flush(String),
    /// Execute the flush with the calculated `min_ts`.
    /// This is an internal command only issued by the `Flush` task.
    FlushWithMinTs(String, TimeStamp),
    /// The command for getting region checkpoints.
    RegionCheckpointsOp(RegionCheckpointOperation),
    /// update global-checkpoint-ts to storage.
    UpdateGlobalCheckpoint(String),
}
```