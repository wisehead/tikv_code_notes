#1.struct TikvConfig

```rust
pub struct TikvConfig {
    #[doc(hidden)]
    #[serde(skip_serializing)]
    #[online_config(hidden)]
    pub cfg_path: String,

    #[doc(hidden)]
    #[online_config(hidden)]
    #[serde(skip_serializing)]
    #[deprecated = "The configuration has been moved to log.level."]
    pub log_level: LogLevel,
    #[doc(hidden)]
    #[online_config(hidden)]
    #[serde(skip_serializing)]
    #[deprecated = "The configuration has been moved to log.file.filename."]
    pub log_file: String,
    #[doc(hidden)]
    #[online_config(hidden)]
    #[serde(skip_serializing)]
    #[deprecated = "The configuration has been moved to log.format."]
    pub log_format: LogFormat,
    #[online_config(hidden)]
    #[serde(skip_serializing)]
    #[deprecated = "The configuration has been moved to log.file.max_days."]
    pub log_rotation_timespan: ReadableDuration,
    #[doc(hidden)]
    #[online_config(hidden)]
    #[serde(skip_serializing)]
    #[deprecated = "The configuration has been moved to log.file.max_size."]
    pub log_rotation_size: ReadableSize,

    #[online_config(skip)]
    pub slow_log_file: String,

    #[online_config(skip)]
    pub slow_log_threshold: ReadableDuration,

    #[online_config(hidden)]
    pub panic_when_unexpected_key_or_data: bool,

    #[doc(hidden)]
    #[serde(skip_serializing)]
    #[online_config(skip)]
    pub enable_io_snoop: bool,

    #[online_config(skip)]
    pub abort_on_panic: bool,

    #[doc(hidden)]
    #[online_config(skip)]
    pub memory_usage_limit: Option<ReadableSize>,

    #[doc(hidden)]
    #[online_config(skip)]
    pub memory_usage_high_water: f64,

    #[online_config(submodule)]
    pub log: LogConfig,

    #[online_config(submodule)]
    pub memory: MemoryConfig,

    #[online_config(submodule)]
    pub quota: QuotaConfig,

    #[online_config(submodule)]
    pub readpool: ReadPoolConfig,

    #[online_config(submodule)]
    pub server: ServerConfig,

    #[online_config(submodule)]
    pub storage: StorageConfig,

    #[online_config(skip)]
    pub pd: PdConfig,

    #[online_config(hidden)]
    pub metric: MetricConfig,

    #[online_config(submodule)]
    #[serde(rename = "raftstore")]
    pub raft_store: RaftstoreConfig,

    #[online_config(submodule)]
    pub coprocessor: CopConfig,

    #[online_config(skip)]
    pub coprocessor_v2: CoprocessorV2Config,

    #[online_config(submodule)]
    pub rocksdb: DbConfig,

    #[online_config(submodule)]
    pub raftdb: RaftDbConfig,

    #[online_config(skip)]
    pub raft_engine: RaftEngineConfig,

    #[online_config(skip)]
    pub security: SecurityConfig,

    #[online_config(submodule)]
    pub import: ImportConfig,

    #[online_config(submodule)]
    pub backup: BackupConfig,

    #[online_config(submodule)]
    // The term "log backup" and "backup stream" are identity.
    // The "log backup" should be the only product name exposed to the user.
    pub log_backup: BackupStreamConfig,

    #[online_config(submodule)]
    pub pessimistic_txn: PessimisticTxnConfig,

    #[online_config(submodule)]
    pub gc: GcConfig,

    #[online_config(submodule)]
    pub split: SplitConfig,

    #[online_config(submodule)]
    pub cdc: CdcConfig,

    #[online_config(submodule)]
    pub resolved_ts: ResolvedTsConfig,

    #[online_config(submodule)]
    pub resource_metering: ResourceMeteringConfig,

    #[online_config(skip)]
    pub causal_ts: CausalTsConfig,

    #[online_config(submodule)]
    pub resource_control: ResourceControlConfig,
}
```