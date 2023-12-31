#1.struct YatpPoolRunner

```rust
pub struct YatpPoolRunner<T: PoolTicker> {
    inner: FutureRunner,
    ticker: TickerWrapper<T>,
    props: Option<GroupProperties>,
    after_start: Option<Arc<dyn Fn() + Send + Sync>>,
    before_stop: Option<Arc<dyn Fn() + Send + Sync>>,
    before_pause: Option<Arc<dyn Fn() + Send + Sync>>,

    // Statistics about the schedule wait duration.
    // local histogram for high,medium,low priority tasks.
    schedule_wait_durations: [LocalHistogram; 3],
    // return the index of `schedule_wait_durations` from task metadata.
    metric_idx_from_task_meta: Arc<dyn Fn(&[u8]) -> usize + Send + Sync>,
}

```