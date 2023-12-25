#1.struct RocksEngine

```rust
#[derive(Clone, Debug)]
pub struct RocksEngine {
    db: Arc<DB>,
    support_multi_batch_write: bool,
    #[cfg(feature = "trace-lifetime")]
    _id: trace::TabletTraceId,
}
```