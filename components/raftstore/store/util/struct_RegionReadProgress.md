#1.struct RegionReadProgress

```rust
/// `RegionReadProgress` is used to keep track of the replica's `safe_ts`, the
/// replica can handle a read request directly without requiring leader lease or
/// read index iff `safe_ts` >= `read_ts` (the `read_ts` is usually stale i.e
/// seconds ago).
///
/// `safe_ts` is updated by the `(apply index, safe ts)` item:
/// ```ignore
/// if self.applied_index >= item.apply_index {
///     self.safe_ts = max(self.safe_ts, item.safe_ts)
/// }
/// ```
///
/// For the leader, the `(apply index, safe ts)` item is publish by the
/// `resolved-ts` worker periodically. For the followers, the item is sync
/// periodically from the leader through the `CheckLeader` rpc.
///
/// The intend is to make the item's `safe ts` larger (more up to date) and
/// `apply index` smaller (require less data)
//
/// TODO: the name `RegionReadProgress` is conflict with the leader lease's
/// `ReadProgress`, should change it to another more proper name
#[derive(Debug)]
pub struct RegionReadProgress {
    // `core` used to keep track and update `safe_ts`, it should
    // not be accessed outside to avoid dead lock
    core: Mutex<RegionReadProgressCore>,
    // The fast path to read `safe_ts` without acquiring the mutex
    // on `core`
    safe_ts: AtomicU64,
}
```