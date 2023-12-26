#1.struct Endpoint

```rust
pub struct Endpoint<S, R, E: KvEngine, PDC> {
    // Note: those fields are more like a shared context between components.
    // For now, we copied them everywhere, maybe we'd better extract them into a
    // context type.
    pub(crate) meta_client: MetadataClient<S>,
    pub(crate) scheduler: Scheduler<Task>,
    pub(crate) store_id: u64,
    pub(crate) regions: R,
    pub(crate) engine: PhantomData<E>,
    pub(crate) pd_client: Arc<PDC>,
    pub(crate) subs: SubscriptionTracer,
    pub(crate) concurrency_manager: ConcurrencyManager,

    // Note: some of fields are public so test cases are able to access them.
    pub range_router: Router,
    observer: BackupStreamObserver,
    pool: Runtime,
    region_operator: RegionSubscriptionManager<S, R, PDC>,
    failover_time: Option<Instant>,
    // We holds the config before, even it is useless for now,
    // however probably it would be useful in the future.
    config: BackupStreamConfig,
    checkpoint_mgr: CheckpointManager,

    // Runtime status:
    /// The handle to abort last save storage safe point.
    /// This is used for simulating an asynchronous background worker.
    /// Each time we spawn a task, once time goes by, we abort that task.
    pub abort_last_storage_save: Option<AbortHandle>,
    pub initial_scan_semaphore: Arc<Semaphore>,
}
```