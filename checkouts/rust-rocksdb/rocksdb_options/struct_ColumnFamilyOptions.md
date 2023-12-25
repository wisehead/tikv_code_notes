#1.struct ColumnFamilyOptions

```rust
pub struct ColumnFamilyOptions {
    pub(crate) inner: *mut Options,
    pub(crate) titan_inner: *mut DBTitanDBOptions,
    env: Option<Arc<Env>>,
    filter: Option<CompactionFilterHandle>,
}
```