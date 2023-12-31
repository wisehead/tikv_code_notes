#1.struct GcWorker

```rust
/// Used to schedule GC operations.
pub struct GcWorker<E>
where
    E: Engine,
{
    engine: E,

    /// Used to signal unsafe destroy range is executed.
    flow_info_sender: Option<Sender<FlowInfo>>,
    region_info_provider: Arc<dyn RegionInfoProvider>,

    config_manager: GcWorkerConfigManager,

    /// How many strong references. The worker will be stopped
    /// once there are no more references.
    refs: Arc<AtomicUsize>,
    worker: Arc<Mutex<LazyWorker<GcTask<E::Local>>>>,
    worker_scheduler: Scheduler<GcTask<E::Local>>,

    gc_manager_handle: Arc<Mutex<Option<GcManagerHandle>>>,
    feature_gate: FeatureGate,
}
```