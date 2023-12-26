#1.trait ConfiguredRaftEngine

```rust
pub trait ConfiguredRaftEngine: RaftEngine {
    fn build(
        _: &TikvConfig,
        _: &Arc<Env>,
        _: &Option<Arc<DataKeyManager>>,
        _: &Cache,
    ) -> (Self, Option<Arc<RocksStatistics>>);
    fn as_rocks_engine(&self) -> Option<&RocksEngine>;
    fn register_config(&self, _cfg_controller: &mut ConfigController);
}
```