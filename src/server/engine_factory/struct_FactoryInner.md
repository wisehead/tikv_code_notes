#1.struct FactoryInner

```rust
struct FactoryInner {
    region_info_accessor: Option<RegionInfoAccessor>,
    rocksdb_config: Arc<DbConfig>,
    api_version: ApiVersion,
    flow_listener: Option<engine_rocks::FlowListener>,
    sst_recovery_sender: Option<Scheduler<String>>,
    encryption_key_manager: Option<Arc<DataKeyManager>>,
    db_resources: DbResources,
    cf_resources: CfResources,
    state_storage: Option<Arc<dyn StateStorage>>,
    lite: bool,
}
```