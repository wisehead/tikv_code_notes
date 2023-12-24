#1.struct PeerFsmDelegate

```rust
pub struct PeerFsmDelegate<'a, EK: KvEngine, ER: RaftEngine, T> {
    pub fsm: &'a mut PeerFsm<EK, ER>,
    pub store_ctx: &'a mut StoreContext<EK, ER, T>,
}
```