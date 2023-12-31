#1.struct ThreadPool

```rust
/// A generic thread pool.
pub struct ThreadPool<T: TaskCell + Send> {
    remote: Remote<T>,
    threads: Mutex<Vec<JoinHandle<()>>>,
}
```