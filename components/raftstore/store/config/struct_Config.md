#1.struct Config

```rust
with_prefix!(prefix_apply "apply-");
with_prefix!(prefix_store "store-");
#[derive(Clone, Debug, Serialize, Deserialize, PartialEq, OnlineConfig)]
#[serde(default)]
#[serde(rename_all = "kebab-case")]
pub struct Config {
    // minimizes disruption when a partitioned node rejoins the cluster by using a two phase
    // election.
    #[online_config(skip)]
    pub prevote: bool,
    #[online_config(skip)]
    pub raftdb_path: String,

    // store capacity. 0 means no limit.
    #[online_config(skip)]
    pub capacity: ReadableSize,

    // raft_base_tick_interval is a base tick interval (ms).
    #[online_config(hidden)]
    pub raft_base_tick_interval: ReadableDuration,
    #[online_config(hidden)]
    pub raft_heartbeat_ticks: usize,
    #[online_config(hidden)]
    pub raft_election_timeout_ticks: usize,
    #[online_config(hidden)]
    pub raft_min_election_timeout_ticks: usize,
    #[online_config(hidden)]
    pub raft_max_election_timeout_ticks: usize,
    pub raft_max_size_per_msg: ReadableSize,
    pub raft_max_inflight_msgs: usize,
    // When the entry exceed the max size, reject to propose it.
    pub raft_entry_max_size: ReadableSize,

    // Interval to compact unnecessary raft log.
    pub raft_log_compact_sync_interval: ReadableDuration,
    // Interval to gc unnecessary raft log.
    pub raft_log_gc_tick_interval: ReadableDuration,
    // A threshold to gc stale raft log, must >= 1.
    pub raft_log_gc_threshold: u64,
    // When entry count exceed this value, gc will be forced trigger.
    pub raft_log_gc_count_limit: Option<u64>,
    // When the approximate size of raft log entries exceed this value,
    // gc will be forced trigger.
    pub raft_log_gc_size_limit: Option<ReadableSize>,
    // Old Raft logs could be reserved if `raft_log_gc_threshold` is not reached.
    // GC them after ticks `raft_log_reserve_max_ticks` times.
    #[doc(hidden)]
    #[online_config(hidden)]
    pub raft_log_reserve_max_ticks: usize,
    // Old logs in Raft engine needs to be purged peridically.
    pub raft_engine_purge_interval: ReadableDuration,
    // When a peer is not responding for this time, leader will not keep entry cache for it.
    pub raft_entry_cache_life_time: ReadableDuration,
    // Deprecated! The configuration has no effect.
    // They are preserved for compatibility check.
    // When a peer is newly added, reject transferring leader to the peer for a while.
    #[doc(hidden)]
    #[serde(skip_serializing)]
    #[online_config(skip)]
    pub raft_reject_transfer_leader_duration: ReadableDuration,

    // Interval (ms) to check region whether need to be split or not.
    pub split_region_check_tick_interval: ReadableDuration,
    /// When size change of region exceed the diff since last check, it
    /// will be checked again whether it should be split.
    pub region_split_check_diff: Option<ReadableSize>,
    /// Interval (ms) to check whether start compaction for a region.
    pub region_compact_check_interval: ReadableDuration,
    /// Number of regions for each time checking.
    pub region_compact_check_step: u64,
    /// Minimum number of tombstones to trigger manual compaction.
    pub region_compact_min_tombstones: u64,
    /// Minimum percentage of tombstones to trigger manual compaction.
    /// Should between 1 and 100.
    pub region_compact_tombstones_percent: u64,
    pub pd_heartbeat_tick_interval: ReadableDuration,
    pub pd_store_heartbeat_tick_interval: ReadableDuration,
    pub snap_mgr_gc_tick_interval: ReadableDuration,
    pub snap_gc_timeout: ReadableDuration,
    pub lock_cf_compact_interval: ReadableDuration,
    pub lock_cf_compact_bytes_threshold: ReadableSize,

    #[online_config(skip)]
    pub notify_capacity: usize,
    pub messages_per_tick: usize,

    /// When a peer is not active for max_peer_down_duration,
    /// the peer is considered to be down and is reported to PD.
    pub max_peer_down_duration: ReadableDuration,

    /// If the leader of a peer is missing for longer than
    /// max_leader_missing_duration, the peer would ask pd to confirm
    /// whether it is valid in any region. If the peer is stale and is not
    /// valid in any region, it will destroy itself.
    pub max_leader_missing_duration: ReadableDuration,
    /// Similar to the max_leader_missing_duration, instead it will log warnings
    /// and try to alert monitoring systems, if there is any.
    pub abnormal_leader_missing_duration: ReadableDuration,
    pub peer_stale_state_check_interval: ReadableDuration,

    #[online_config(hidden)]
    pub leader_transfer_max_log_lag: u64,

    #[online_config(skip)]
    pub snap_apply_batch_size: ReadableSize,

    // used to periodically check whether schedule pending applies in region runner
    #[doc(hidden)]
    #[online_config(skip)]
    pub region_worker_tick_interval: ReadableDuration,

    // used to periodically check whether we should delete a stale peer's range in
    // region runner
    #[doc(hidden)]
    #[online_config(skip)]
    pub clean_stale_ranges_tick: usize,

    // Interval (ms) to check region whether the data is consistent.
    pub consistency_check_interval: ReadableDuration,

    #[online_config(hidden)]
    pub report_region_flow_interval: ReadableDuration,

    // The lease provided by a successfully proposed and applied entry.
    pub raft_store_max_leader_lease: ReadableDuration,

    // Interval of scheduling a tick to check the leader lease.
    // It will be set to raft_store_max_leader_lease/4 by default.
    pub check_leader_lease_interval: ReadableDuration,

    // Check if leader lease will expire at `current_time + renew_leader_lease_advance_duration`.
    // It will be set to raft_store_max_leader_lease/4 by default.
    pub renew_leader_lease_advance_duration: ReadableDuration,

    // Right region derive origin region id when split.
    #[online_config(hidden)]
    pub right_derive_when_split: bool,

    /// This setting can only ensure conf remove will not be proposed by the
    /// peer being removed. But it can't guarantee the remove is applied
    /// when the target is not leader. That means we always need to check if
    /// it's working as expected when a leader applies a self-remove conf
    /// change. Keep the configuration only for convenient test.
    #[cfg(any(test, feature = "testexport"))]
    pub allow_remove_leader: bool,

    /// Max log gap allowed to propose merge.
    #[online_config(hidden)]
    pub merge_max_log_gap: u64,
    /// Interval to re-propose merge.
    pub merge_check_tick_interval: ReadableDuration,

    #[online_config(hidden)]
    pub use_delete_range: bool,

    #[online_config(skip)]
    pub snap_generator_pool_size: usize,

    pub cleanup_import_sst_interval: ReadableDuration,

    /// Maximum size of every local read task batch.
    pub local_read_batch_size: u64,

    #[online_config(submodule)]
    #[serde(flatten, with = "prefix_apply")]
    pub apply_batch_system: BatchSystemConfig,

    #[online_config(submodule)]
    #[serde(flatten, with = "prefix_store")]
    pub store_batch_system: BatchSystemConfig,

    /// If it is 0, it means io tasks are handled in store threads.
    #[online_config(skip)]
    pub store_io_pool_size: usize,

    #[online_config(skip)]
    pub store_io_notify_capacity: usize,

    #[online_config(skip)]
    pub future_poll_size: usize,
    #[online_config(skip)]
    pub hibernate_regions: bool,
    #[doc(hidden)]
    #[online_config(hidden)]
    pub dev_assert: bool,
    #[online_config(hidden)]
    pub apply_yield_duration: ReadableDuration,

    #[serde(with = "perf_level_serde")]
    #[online_config(skip)]
    pub perf_level: PerfLevel,

    #[doc(hidden)]
    #[online_config(skip)]
    /// Disable this feature by set to 0, logic will be removed in other pr.
    /// When TiKV memory usage reaches `memory_usage_high_water` it will try to
    /// limit memory increasing. For raftstore layer entries will be evicted
    /// from entry cache, if they utilize memory more than
    /// `evict_cache_on_memory_ratio` * total.
    ///
    /// Set it to 0 can disable cache evict.
    // By default it's 0.2. So for different system memory capacity, cache evict happens:
    // * system=8G,  memory_usage_limit=6G,  evict=1.2G
    // * system=16G, memory_usage_limit=12G, evict=2.4G
    // * system=32G, memory_usage_limit=24G, evict=4.8G
    pub evict_cache_on_memory_ratio: f64,

    pub cmd_batch: bool,

    /// When the count of concurrent ready exceeds this value, command will not
    /// be proposed until the previous ready has been persisted.
    /// If `cmd_batch` is 0, this config will have no effect.
    /// If it is 0, it means no limit.
    pub cmd_batch_concurrent_ready_max_count: usize,

    /// When the size of raft db writebatch exceeds this value, write will be
    /// triggered.
    pub raft_write_size_limit: ReadableSize,

    pub waterfall_metrics: bool,

    pub io_reschedule_concurrent_max_count: usize,
    pub io_reschedule_hotpot_duration: ReadableDuration,

    // Deprecated! Batch is done in raft client.
    #[doc(hidden)]
    #[serde(skip_serializing)]
    #[online_config(skip)]
    pub raft_msg_flush_interval: ReadableDuration,

    // Deprecated! These configuration has been moved to Coprocessor.
    // They are preserved for compatibility check.
    #[doc(hidden)]
    #[serde(skip_serializing)]
    #[online_config(skip)]
    pub region_max_size: ReadableSize,
    #[doc(hidden)]
    #[serde(skip_serializing)]
    #[online_config(skip)]
    pub region_split_size: ReadableSize,
    // Deprecated! The time to clean stale peer safely can be decided based on RocksDB snapshot
    // sequence number.
    #[doc(hidden)]
    #[serde(skip_serializing)]
    #[online_config(skip)]
    pub clean_stale_peer_delay: ReadableDuration,

    // Interval to inspect the latency of raftstore for slow store detection.
    pub inspect_interval: ReadableDuration,

    // Interval to report min resolved ts, if it is zero, it means disabled.
    pub report_min_resolved_ts_interval: ReadableDuration,

    /// Interval to check whether to reactivate in-memory pessimistic lock after
    /// being disabled before transferring leader.
    pub reactive_memory_lock_tick_interval: ReadableDuration,
    /// Max tick count before reactivating in-memory pessimistic lock.
    pub reactive_memory_lock_timeout_tick: usize,
    // Interval of scheduling a tick to report region buckets.
    pub report_region_buckets_tick_interval: ReadableDuration,

    /// Interval to check long uncommitted proposals.
    #[doc(hidden)]
    pub check_long_uncommitted_interval: ReadableDuration,
    /// Base threshold of long uncommitted proposal.
    #[doc(hidden)]
    pub long_uncommitted_base_threshold: ReadableDuration,

    #[doc(hidden)]
    pub max_snapshot_file_raw_size: ReadableSize,

    pub unreachable_backoff: ReadableDuration,
}


```