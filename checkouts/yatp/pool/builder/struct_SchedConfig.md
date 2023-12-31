#1.struct SchedConfig

```rust
/// Configuration for schedule algorithm.
pub struct SchedConfig {
    /// The maximum number of running threads at the same time.
    pub max_thread_count: usize,
    /// The core number of running threads at the same time. It
    /// is defined as the AtomicUsize because it needs to be used
    /// to scale the number of workers at runtime, and the adjustable
    /// range is min_thread_count to max_thread_count.
    pub core_thread_count: AtomicUsize,
    /// The minimum number of running threads at the same time.
    pub min_thread_count: usize,
    /// The maximum tries to rerun an unfinished task before pushing
    /// back to queue.
    pub max_inplace_spin: usize,
    /// The maximum allowed idle time for a thread. Thread will only be
    /// woken up when algorithm thinks it needs more worker.
    pub max_idle_time: Duration,
    /// The maximum time to wait for a task before increasing the
    /// running thread slots.
    pub max_wait_time: Duration,
    /// The minimum interval between waking a thread.
    pub wake_backoff: Duration,
    /// The minimum interval between increasing running threads.
    pub alloc_slot_backoff: Duration,
}
```