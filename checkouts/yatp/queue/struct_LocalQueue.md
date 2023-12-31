#1.struct LocalQueue

```rust
/// The local queue of a task queue.
pub(crate) struct LocalQueue<T>(LocalQueueInner<T>);

```