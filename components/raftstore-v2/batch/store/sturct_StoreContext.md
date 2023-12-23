#1.struct StoreContext

```rust
/// A per-thread context shared by the [`StoreFsm`] and multiple [`PeerFsm`]s.
pub struct StoreContext<EK: KvEngine, ER: RaftEngine, T> {
    /// A logger without any KV. It's clean for creating new PeerFSM.
    pub logger: Logger,
    /// The transport for sending messages to peers on other stores.
    pub trans: T,
    pub current_time: Option<Timespec>,
    pub has_ready: bool,
    pub raft_metrics: RaftMetrics,
    /// The latest configuration.
    pub cfg: Config,
    pub router: StoreRouter<EK, ER>,
    /// The tick batch for delay ticking. It will be flushed at the end of every
    /// round.
    pub tick_batch: Vec<PeerTickBatch>,
    /// The precise timer for scheduling tick.
    pub timer: SteadyTimer,
    pub write_senders: WriteSenders<EK, ER>,
    pub engine: ER,
    pub tablet_factory: Arc<dyn TabletFactory<EK>>,
    pub log_fetch_scheduler: Scheduler<RaftlogFetchTask>,
}
```