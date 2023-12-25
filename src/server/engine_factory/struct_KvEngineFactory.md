#1.struct KvEngineFactory

```rust
pub struct KvEngineFactory {
    inner: Arc<FactoryInner>,
    compact_event_sender: Option<Arc<dyn CompactedEventSender + Send + Sync>>,
}
```