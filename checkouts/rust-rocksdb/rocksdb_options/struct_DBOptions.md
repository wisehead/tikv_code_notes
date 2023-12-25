#1.struct DBOptions

```rust
pub struct DBOptions {
    pub(crate) inner: *mut Options,
    env: Option<Arc<Env>>,
    pub(crate) titan_inner: *mut DBTitanDBOptions,
}
```