#1.struct BackupStreamObserver

```rust
/// An Observer for Backup Stream.
///
/// It observes raftstore internal events, such as:
///   1. Apply command events.
#[derive(Clone)]
pub struct BackupStreamObserver {
    scheduler: Scheduler<Task>,
    // Note: maybe wrap those fields to methods?
    pub ranges: Arc<RwLock<SegmentSet<Vec<u8>>>>,
}
```