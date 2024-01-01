#1.struct StreamTaskInfo

```rust
pub struct StreamTaskInfo {
    pub(crate) task: StreamTask,
    /// support external storage. eg local/s3.
    pub(crate) storage: Arc<dyn ExternalStorage>,
    /// The listening range of the task.
    ranges: Vec<(Vec<u8>, Vec<u8>)>,
    /// The temporary file index. Both meta (m prefixed keys) and data (t
    /// prefixed keys).
    files: SlotMap<TempFileKey, DataFile>,
    /// flushing_files contains files pending flush.
    flushing_files: RwLock<Vec<(TempFileKey, DataFile, DataFileInfo)>>,
    /// flushing_meta_files contains meta files pending flush.
    flushing_meta_files: RwLock<Vec<(TempFileKey, DataFile, DataFileInfo)>>,
    /// last_flush_ts represents last time this task flushed to storage.
    last_flush_time: AtomicPtr<Instant>,
    /// The min resolved TS of all regions involved.
    min_resolved_ts: TimeStamp,
    /// Total size of all temporary files in byte.
    total_size: AtomicUsize,
    /// This should only be set to `true` by `compare_and_set(current=false,
    /// value=true)`. The thread who setting it to `true` takes the
    /// responsibility of sending the request to the scheduler for flushing
    /// the files then.
    ///
    /// If the request failed, that thread can set it to `false` back then.
    flushing: AtomicBool,
    /// This counts how many times this task has failed to flush.
    flush_fail_count: AtomicUsize,
    /// global checkpoint ts for this task.
    global_checkpoint_ts: AtomicU64,
    /// The size limit of the merged file for this task.
    merged_file_size_limit: u64,
    /// The pool for holding the temporary files.
    temp_file_pool: Arc<TempFilePool>,
}
```