#1.struct CachedEntries

```rust
/// Committed entries sent to apply threads.
#[derive(Clone)]
pub struct CachedEntries {
    pub range: Range<u64>,
    // Entries and dangle size for them. `dangle` means not in entry cache.
    entries: Arc<Mutex<(Vec<Entry>, usize)>>,
}
```