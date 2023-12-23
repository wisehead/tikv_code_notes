#1.struct StoreFsmDelegate

```rust
pub struct StoreFsmDelegate<'a, EK: KvEngine, ER: RaftEngine, T> {
    fsm: &'a mut StoreFsm,
    store_ctx: &'a mut StoreContext<EK, ER, T>,
}
```