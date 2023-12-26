#1.struct TikvEngines

```rust
struct TikvEngines<EK: KvEngine, ER: RaftEngine> {
    engines: Engines<EK, ER>,
    store_meta: Arc<Mutex<StoreMeta>>,
    engine: RaftKv<EK, ServerRaftStoreRouter<EK, ER>>,
}
```