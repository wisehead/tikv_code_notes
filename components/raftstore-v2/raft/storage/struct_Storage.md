#1.struct Storage

```rust
/// A storage for raft.
///
/// It's similar to `PeerStorage` in v1.
pub struct Storage<ER> {
    entry_storage: EntryStorage<ER>,
    peer: metapb::Peer,
    region_state: RegionLocalState,
    /// Whether states has been persisted before. If a peer is just created by
    /// by messages, it has not persisted any states, we need to persist them
    /// at least once dispite whether the state changes since create.
    ever_persisted: bool,
    logger: Logger,
}
```