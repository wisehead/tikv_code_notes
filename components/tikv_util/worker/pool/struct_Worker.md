#1.struct Worker

```rust
/// A worker that can schedule time consuming tasks.
#[derive(Clone)]
pub struct Worker {
    pool: Arc<Mutex<Option<ThreadPool<yatp::task::future::TaskCell>>>>,
    remote: Remote<yatp::task::future::TaskCell>,
    pending_capacity: usize,
    counter: Arc<AtomicUsize>,
    stop: Arc<AtomicBool>,
    thread_count: usize,
}
```