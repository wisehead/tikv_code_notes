#1.struct SnapManagerCore

```rust
struct SnapManagerCore {
    // directory to store snapfile.
    base: String,

    registry: Arc<RwLock<HashMap<SnapKey, Vec<SnapEntry>>>>,
    limiter: Limiter,
    temp_sst_id: Arc<AtomicU64>,
    encryption_key_manager: Option<Arc<DataKeyManager>>,
    max_per_file_size: Arc<AtomicU64>,
    enable_multi_snapshot_files: Arc<AtomicBool>,
    stats: Arc<Mutex<Vec<SnapshotStat>>>,
}
```