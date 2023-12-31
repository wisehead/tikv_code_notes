#1.struct GcConfig

```rust
#[derive(Clone, Debug, Serialize, Deserialize, PartialEq, OnlineConfig)]
#[serde(default)]
#[serde(rename_all = "kebab-case")]
pub struct GcConfig {
    pub ratio_threshold: f64,
    pub batch_keys: usize,
    pub max_write_bytes_per_sec: ReadableSize,
    pub enable_compaction_filter: bool,
    /// By default compaction_filter can only works if `cluster_version` is
    /// greater than 5.0.0. Change `compaction_filter_skip_version_check`
    /// can enable it by force.
    pub compaction_filter_skip_version_check: bool,
}
```