#1.struct EntryStorage

```rust
/// A subset of `PeerStorage` that focus on accessing log entries.
pub struct EntryStorage<ER> {
    region_id: u64,
    peer_id: u64,
    raft_engine: ER,
    cache: EntryCache,
    raft_state: RaftLocalState,
    apply_state: RaftApplyState,
    last_term: u64,
    applied_term: u64,
    raftlog_fetch_scheduler: Scheduler<RaftlogFetchTask>,
    raftlog_fetch_stats: AsyncFetchStats,
    async_fetch_results: RefCell<HashMap<u64, RaftlogFetchState>>,
}
```