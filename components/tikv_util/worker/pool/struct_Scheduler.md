#1.struct Scheduler

```rust
/// Scheduler provides interface to schedule task to underlying workers.
pub struct Scheduler<T: Display + Send> {
    counter: Arc<AtomicUsize>,
    sender: UnboundedSender<Msg<T>>,
    pending_capacity: usize,
    metrics_pending_task_count: IntGauge,
}
```