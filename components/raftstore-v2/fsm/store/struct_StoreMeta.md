#1.struct StoreMeta

```rust
pub struct StoreMeta<E>
where
    E: KvEngine,
{
    pub store_id: Option<u64>,
    /// region_id -> reader
    pub readers: HashMap<u64, ReadDelegate>,
    /// region_id -> tablet cache
    pub tablet_caches: HashMap<u64, CachedTablet<E>>,
}
```