#1.struct BackupStreamConfig

```rust
#[derive(Clone, Serialize, Deserialize, PartialEq, Debug, OnlineConfig)]
#[serde(default)]
#[serde(rename_all = "kebab-case")]
pub struct BackupStreamConfig {
    #[online_config(skip)]
    pub min_ts_interval: ReadableDuration,

    pub max_flush_interval: ReadableDuration,
    #[online_config(skip)]
    pub num_threads: usize,
    #[online_config(skip)]
    pub enable: bool,
    #[online_config(skip)]
    pub temp_path: String,

    pub file_size_limit: ReadableSize,

    #[doc(hidden)]
    #[serde(skip_serializing)]
    #[online_config(skip)]
    // Let's hide this config for now.
    pub temp_file_memory_quota: ReadableSize,

    #[online_config(skip)]
    pub initial_scan_pending_memory_quota: ReadableSize,
    #[online_config(skip)]
    pub initial_scan_rate_limit: ReadableSize,
    pub initial_scan_concurrency: usize,
}
```