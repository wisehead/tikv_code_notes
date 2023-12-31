#1.struct EngineFlowController

```rust
/// Flow controller is used to throttle the write rate at scheduler level,
/// aiming to substitute the write stall mechanism of RocksDB. It features in
/// two points:
///   * throttle at scheduler, so raftstore and apply won't be blocked anymore
///   * better control on the throttle rate to avoid QPS drop under heavy write
///
/// When write stall happens, the max speed of write rate max_delayed_write_rate
/// is limited to 16MB/s by default which doesn't take real disk ability into
/// account. It may underestimate the disk's throughout that 16MB/s is too small
/// at once, causing a very large jitter on the write duration.
/// Also, it decreases the delayed write rate further if the factors still
/// exceed the threshold. So under heavy write load, the write rate may be
/// throttled to a very low rate from time to time, causing QPS drop eventually.

/// For compaction pending bytes, we use discardable ratio to do flow control
/// which is separated mechanism from throttle speed. Compaction pending bytes
/// is a approximate value, usually, changes up and down dramatically, so it's
/// unwise to map compaction pending bytes to a specified throttle speed.
/// Instead, mapping it from soft limit to hard limit as 0% to 100% discardable
/// ratio. With this, there must be a point that foreground write rate is equal
/// to the background compaction pending bytes consuming rate so that compaction
/// pending bytes is kept around a steady level.
///
/// Here is a brief flow showing where the mechanism works:
/// grpc -> check should drop(discardable ratio) -> limiter -> async write to
/// raftstore
pub struct EngineFlowController {
    discard_ratio: Arc<AtomicU32>,
    limiter: Arc<Limiter>,
    enabled: Arc<AtomicBool>,
    tx: Option<SyncSender<Msg>>,
    handle: Option<std::thread::JoinHandle<()>>,
}

```