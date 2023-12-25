#1.struct ServerTransport

```rust
pub struct ServerTransport<T, S>
where
    T: RaftExtension + 'static,
    S: StoreAddrResolver + 'static,
{
    raft_client: RaftClient<S, T>,
}

```