#1.struct Local

```rust
/// Spawns tasks to the associated thread pool.
///
/// It's different from `Remote` because it submits tasks to the local queue
/// instead of global queue, so new tasks can take advantage of cache
/// coherence.
pub struct Local<T> {
    id: usize,
    local_queue: LocalQueue<T>,
    core: Arc<QueueCore<T>>,
}
```