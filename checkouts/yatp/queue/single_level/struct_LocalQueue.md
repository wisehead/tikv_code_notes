#1.struct LocalQueue

```rust
/// The local queue of a single level work stealing task queue.
pub struct LocalQueue<T> {
    local_queue: Worker<T>,
    injector: Arc<Injector<T>>,
    stealers: Vec<Stealer<T>>,
}

```