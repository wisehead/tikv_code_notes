#1.struct MemTable
```rust
/// In-memory storage for Raft Groups.
///
/// Each Raft Group has its own `MemTable` to store all key value pairs and the
/// file locations of all log entries.
pub struct MemTable<A: AllocatorTrait> {
    /// The ID of current Raft Group.
    region_id: u64,

    /// Container of entries. Incoming entries are pushed to the back with
    /// ascending log indexes.
    #[cfg(feature = "swap")]
    entry_indexes: VecDeque<ThinEntryIndex, A>,
    #[cfg(not(feature = "swap"))]
    entry_indexes: VecDeque<ThinEntryIndex>,
    /// The log index of the first entry.
    first_index: u64,
    /// The amount of rewritten entries. Rewritten entries are the oldest
    /// entries and stored at the front of the container.
    rewrite_count: usize,

    /// A map of active key value pairs.
    kvs: BTreeMap<Vec<u8>, (Vec<u8>, FileId)>,

    /// Shared statistics.
    global_stats: Arc<GlobalStats>,

    _phantom: PhantomData<A>,
}
```