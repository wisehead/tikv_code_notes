#1.struct EntryCache

```rust
struct EntryCache {
    // The last index of persisted entry.
    // It should be equal to `RaftLog::persisted`.
    persisted: u64,
    cache: VecDeque<Entry>,
    trace: VecDeque<CachedEntries>,
    hit: Cell<u64>,
    miss: Cell<u64>,
    #[cfg(test)]
    size_change_cb: Option<Box<dyn Fn(i64) + Send + 'static>>,
}
```