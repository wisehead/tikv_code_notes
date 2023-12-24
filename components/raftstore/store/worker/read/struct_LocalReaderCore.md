#1.struct LocalReaderCore

```rust
/// #[RaftstoreCommon]: LocalReader is an entry point where local read requests are dipatch to the
/// relevant regions by LocalReader so that these requests can be handled by the
/// relevant ReadDelegate respectively.
pub struct LocalReaderCore<D, S> {
    pub store_id: Cell<Option<u64>>,
    store_meta: S,
    pub delegates: LruCache<u64, D>,
}
```