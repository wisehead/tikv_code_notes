#1.struct LocalReader

```rust
pub struct LocalReader<E, C>
where
    E: KvEngine,
    C: ProposalRouter<E::Snapshot> + CasualRouter<E>,
{
    local_reader: LocalReaderCore<CachedReadDelegate<E>, StoreMetaDelegate<E>>,
    kv_engine: E,
    snap_cache: SnapCache<E>,
    // A channel to raftstore.
    router: C,
}
```