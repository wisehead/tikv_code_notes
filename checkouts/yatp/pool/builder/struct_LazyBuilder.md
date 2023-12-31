#1.struct LazyBuilder

```rust
/// A builder for lazy spawning.
pub struct LazyBuilder<T> {
    builder: Builder,
    core: Arc<QueueCore<T>>,
    local_queues: Vec<LocalQueue<T>>,
}
```