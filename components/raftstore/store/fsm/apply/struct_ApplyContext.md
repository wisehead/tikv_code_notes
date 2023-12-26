#1.struct ApplyContext

```rust
struct ApplyContext<EK>
where
    EK: KvEngine,
{
    tag: String,
    timer: Option<Instant>,
    host: CoprocessorHost<EK>,
    importer: Arc<SstImporter>,
    region_scheduler: Scheduler<RegionTask<EK::Snapshot>>,
    router: ApplyRouter<EK>,
    notifier: Box<dyn Notifier<EK>>,
    engine: EK,
    applied_batch: ApplyCallbackBatch<EK::Snapshot>,
    apply_res: Vec<ApplyRes<EK::Snapshot>>,
    exec_log_index: u64,
    exec_log_term: u64,

    kv_wb: EK::WriteBatch,
    kv_wb_last_bytes: u64,
    kv_wb_last_keys: u64,

    committed_count: usize,

    // Whether synchronize WAL is preferred.
    sync_log_hint: bool,
    // Whether to use the delete range API instead of deleting one by one.
    use_delete_range: bool,

    perf_context: EK::PerfContext,

    yield_duration: Duration,
    yield_msg_size: u64,

    store_id: u64,
    /// region_id -> (peer_id, is_splitting)
    /// Used for handling race between splitting and creating new peer.
    /// An uninitialized peer can be replaced to the one from splitting iff they
    /// are exactly the same peer.
    pending_create_peers: Arc<Mutex<HashMap<u64, (u64, bool)>>>,

    /// We must delete the ingested file before calling `callback` so that any
    /// ingest-request reaching this peer could see this update if leader
    /// had changed. We must also delete them after the applied-index
    /// has been persisted to kvdb because this entry may replay because of
    /// panic or power-off, which happened before `WriteBatch::write` and
    /// after `SstImporter::delete`. We shall make sure that this entry will
    /// never apply again at first, then we can delete the ssts files.
    delete_ssts: Vec<SstMetaInfo>,

    /// A self-defined engine may be slow to ingest ssts.
    /// It may move some elements of `delete_ssts` into `pending_delete_ssts` to
    /// delay deletion. Otherwise we may lost data.
    pending_delete_ssts: Vec<SstMetaInfo>,

    /// The priority of this Handler.
    priority: Priority,
    /// Whether to yield high-latency operation to low-priority handler.
    yield_high_latency_operation: bool,

    /// The ssts waiting to be ingested in `write_to_db`.
    pending_ssts: Vec<SstMetaInfo>,

    /// The pending inspector should be cleaned at the end of a write.
    pending_latency_inspect: Vec<LatencyInspector>,
    apply_wait: LocalHistogram,
    apply_time: LocalHistogram,

    key_buffer: Vec<u8>,

    // Whether to disable WAL.
    disable_wal: bool,

    /// A general apply progress for a delegate is:
    /// `prepare_for` -> `commit` [-> `commit` ...] -> `finish_for`.
    /// Sometimes an `ApplyRes` is created with an applied_index, but data
    /// before the applied index is still not written to kvdb. Let's call the
    /// `ApplyRes` uncommitted. Data will finally be written to kvdb in
    /// `flush`.
    uncommitted_res_count: usize,

    enable_v2_compatible_learner: bool,
}

```