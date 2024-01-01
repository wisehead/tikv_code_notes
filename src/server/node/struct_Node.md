#1.struct Node

```rust
/// A wrapper for the raftstore which runs Multi-Raft.
// TODO: we will rename another better name like RaftStore later.
pub struct Node<C: PdClient + 'static, EK: KvEngine, ER: RaftEngine> {
    cluster_id: u64,
    store: metapb::Store,
    store_cfg: Arc<VersionTrack<StoreConfig>>,
    api_version: ApiVersion,
    system: RaftBatchSystem<EK, ER>,
    has_started: bool,

    pd_client: Arc<C>,
    state: Arc<Mutex<GlobalReplicationState>>,
    bg_worker: Worker,
    health_service: Option<HealthService>,
}
```