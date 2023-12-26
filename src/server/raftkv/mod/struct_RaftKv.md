#1.struct RaftKv

```rust
pub struct RaftKv<E, S>
where
    E: KvEngine,
    S: RaftStoreRouter<E> + LocalReadRouter<E> + 'static,
{
    router: RaftRouterWrap<S, E>,
    engine: E,
    txn_extra_scheduler: Option<Arc<dyn TxnExtraScheduler>>,
    region_leaders: Arc<RwLock<HashSet<u64>>>,
}
```