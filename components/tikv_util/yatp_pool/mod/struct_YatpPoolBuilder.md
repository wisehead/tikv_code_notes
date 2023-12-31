#1.struct YatpPoolBuilder

```rust
pub struct YatpPoolBuilder<T: PoolTicker> {
    name_prefix: Option<String>,
    ticker: TickerWrapper<T>,
    after_start: Option<Arc<dyn Fn() + Send + Sync>>,
    before_stop: Option<Arc<dyn Fn() + Send + Sync>>,
    before_pause: Option<Arc<dyn Fn() + Send + Sync>>,
    min_thread_count: usize,
    core_thread_count: usize,
    max_thread_count: usize,
    stack_size: usize,
    max_tasks: usize,
    cleanup_method: CleanupMethod,

    // whether to tracker task scheduling wait duration
    enable_task_wait_metrics: bool,
    metric_idx_from_task_meta: Option<Arc<dyn Fn(&[u8]) -> usize + Send + Sync>>,

    #[cfg(test)]
    background_cleanup_hook: Option<Arc<dyn Fn() + Send + Sync>>,
}
```