#1.struct ServerRaftStoreRouter

```rust
/// A router that routes messages to the raftstore
pub struct ServerRaftStoreRouter<EK, ER>
where
    EK: KvEngine,
    ER: RaftEngine,
{
    router: RaftRouter<EK, ER>,
    local_reader: LocalReader<EK, RaftRouter<EK, ER>>,
}

```