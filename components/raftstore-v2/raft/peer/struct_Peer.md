#1.struct Peer

```rust
/// A peer that delegates commands between state machine and raft.
pub struct Peer<EK: KvEngine, ER: RaftEngine> {
    raft_group: RawNode<Storage<EK, ER>>,
    tablet: CachedTablet<EK>,
    tablet_being_flushed: bool,

    /// Statistics for self.
    self_stat: PeerStat,

    /// We use a cache for looking up peers. Not all peers exist in region's
    /// peer list, for example, an isolated peer may need to send/receive
    /// messages with unknown peers after recovery.
    peer_cache: Vec<metapb::Peer>,
    /// Statistics for other peers, only maintained when self is the leader.
    peer_heartbeats: HashMap<u64, Instant>,

    /// For raft log compaction.
    compact_log_context: CompactLogContext,

    merge_context: Option<Box<MergeContext>>,
    last_sent_snapshot_index: u64,

    /// Encoder for batching proposals and encoding them in a more efficient way
    /// than protobuf.
    raw_write_encoder: Option<SimpleWriteReqEncoder>,
    proposals: ProposalQueue<Vec<CmdResChannel>>,
    apply_scheduler: Option<ApplyScheduler>,

    /// Set to true if any side effect needs to be handled.
    has_ready: bool,
    /// Sometimes there is no ready at all, but we need to trigger async write.
    has_extra_write: bool,
    replay_watch: Option<Arc<ReplayWatch>>,
    /// Writer for persisting side effects asynchronously.
    pub(crate) async_writer: AsyncWriter<EK, ER>,

    destroy_progress: DestroyProgress,

    pub(crate) logger: Logger,
    pending_reads: ReadIndexQueue<QueryResChannel>,
    read_progress: Arc<RegionReadProgress>,
    leader_lease: Lease,

    region_buckets_info: BucketStatsInfo,

    /// Transaction extensions related to this peer.
    txn_context: TxnContext,

    pending_ticks: Vec<PeerTick>,

    /// Check whether this proposal can be proposed based on its epoch.
    proposal_control: ProposalControl,

    // Trace which peers have not finished split.
    split_trace: Vec<(u64, HashSet<u64>)>,
    split_flow_control: SplitFlowControl,
    /// `MsgAppend` messages from newly split leader should be step after peer
    /// steps snapshot from split, otherwise leader may send an unnecessary
    /// snapshot. So the messages are recorded temporarily and will be handled
    /// later.
    split_pending_append: SplitPendingAppend,

    /// Apply related State changes that needs to be persisted to raft engine.
    ///
    /// To make recovery correct, we need to persist all state changes before
    /// advancing apply index.
    state_changes: Option<Box<ER::LogBatch>>,
    flush_state: Arc<FlushState>,
    sst_apply_state: SstApplyState,

    /// lead_transferee if this peer(leader) is in a leadership transferring.
    leader_transferee: u64,

    long_uncommitted_threshold: u64,

    /// Pending messages to be sent on handle ready. We should avoid sending
    /// messages immediately otherwise it may break the persistence assumption.
    pending_messages: Vec<RaftMessage>,

    gc_peer_context: GcPeerContext,

    abnormal_peer_context: AbnormalPeerContext,
}

```