#1.struct Runner

```rust
pub struct Runner<EK, R, T>
where
    EK: KvEngine,
    T: PdClient + 'static,
{
    batch_size: usize,
    use_delete_range: bool,
    ingest_copy_symlink: bool,
    clean_stale_tick: usize,
    clean_stale_check_interval: Duration,
    clean_stale_ranges_tick: usize,

    tiflash_stores: HashMap<u64, bool>,
    // we may delay some apply tasks if level 0 files to write stall threshold,
    // pending_applies records all delayed apply task, and will check again later
    pending_applies: VecDeque<Task<EK::Snapshot>>,
    // Ranges that have been logically destroyed at a specific sequence number. We can
    // assume there will be no reader (engine snapshot) newer than that sequence number. Therefore,
    // they can be physically deleted with `DeleteFiles` when we're sure there is no older
    // reader as well.
    // To protect this assumption, before a new snapshot is applied, the overlapping pending ranges
    // must first be removed.
    // The sole purpose of maintaining this list is to optimize deletion with `DeleteFiles`
    // whenever we can. Errors while processing them can be ignored.
    pending_delete_ranges: PendingDeleteRanges,

    engine: EK,
    mgr: SnapManager,
    coprocessor_host: CoprocessorHost<EK>,
    router: R,
    pd_client: Option<Arc<T>>,
    pool: ThreadPool<TaskCell>,
}


```