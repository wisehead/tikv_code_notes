#1.struct Resolver

```rust
// Resolver resolves timestamps that guarantee no more commit will happen before
// the timestamp.
pub struct Resolver {
    region_id: u64,
    // key -> start_ts
    locks_by_key: HashMap<Arc<[u8]>, TimeStamp>,
    // start_ts -> locked keys.
    lock_ts_heap: BTreeMap<TimeStamp, TxnLocks>,
    // The last shrink time.
    last_aggressive_shrink_time: Instant,
    // The timestamps that guarantees no more commit will happen before.
    resolved_ts: TimeStamp,
    // The highest index `Resolver` had been tracked
    tracked_index: u64,
    // The region read progress used to utilize `resolved_ts` to serve stale read request
    read_progress: Option<Arc<RegionReadProgress>>,
    // The timestamps that advance the resolved_ts when there is no more write.
    min_ts: TimeStamp,
    // Whether the `Resolver` is stopped
    stopped: bool,
    // The memory quota for the `Resolver` and its lock keys and timestamps.
    memory_quota: Arc<MemoryQuota>,
    // The last attempt of resolve(), used for diagnosis.
    last_attempt: Option<LastAttempt>,
}
```