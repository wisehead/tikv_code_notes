#1.struct RegionReadProgressCore

```rust

pub struct RegionReadProgressCore {
    peer_id: u64,
    region_id: u64,
    applied_index: u64,
    // A wrapper of `(apply_index, safe_ts)` item, where the `read_state.ts` is the peer's current
    // `safe_ts` and the `read_state.idx` is the smallest `apply_index` required for that `safe_ts`
    read_state: ReadState,
    // The local peer's acknowledge about the leader
    leader_info: LocalLeaderInfo,
    // `pending_items` is a *sorted* list of `(apply_index, safe_ts)` item
    pending_items: VecDeque<ReadState>,
    // After the region commit merged, the region's key range is extended and the region's
    // `safe_ts` should reset to `min(source_safe_ts, target_safe_ts)`, and start reject stale
    // `read_state` item with index smaller than `last_merge_index` to avoid `safe_ts` undo the
    // decrease
    last_merge_index: u64,
    // Stop update `safe_ts`
    pause: bool,
    // Discard incoming `(idx, ts)`
    discard: bool,
    // A notify to trigger advancing resolved ts immediately.
    advance_notify: Option<Arc<Notify>>,
    // The approximate last instant of calling update_safe_ts(), used for diagnosis.
    // Only the update from advance of resolved-ts is counted. Other sources like CDC or
    // backup-stream are ignored.
    last_instant_of_update_safe_ts: Option<Instant>,
    last_instant_of_consume_leader: Option<Instant>,
}
```