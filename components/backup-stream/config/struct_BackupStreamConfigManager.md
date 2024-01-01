#1.struct BackupStreamConfigManager

```rust
pub struct BackupStreamConfigManager {
    pub scheduler: Scheduler<Task>,
    pub config: Arc<RwLock<BackupStreamConfig>>,
}
```