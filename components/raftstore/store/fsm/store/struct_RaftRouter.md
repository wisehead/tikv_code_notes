#1.struct RaftRouter

```rust
pub struct RaftRouter<EK, ER>
where
    EK: KvEngine,
    ER: RaftEngine,
{
    pub router: BatchRouter<PeerFsm<EK, ER>, StoreFsm<EK>>,
}
```