#1.trait Transport

```rust
/// Transports messages between different Raft peers.
pub trait Transport: Send + Clone {
    fn send(&mut self, msg: RaftMessage) -> Result<()>;

    // empty list means all stores are allowed to send.
    fn set_store_allowlist(&mut self, stores: Vec<u64>);

    fn need_flush(&self) -> bool;

    fn flush(&mut self);
}
```