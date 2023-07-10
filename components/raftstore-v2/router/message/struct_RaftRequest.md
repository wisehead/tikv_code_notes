#1.struct RaftRequest

```rust
pub struct RaftRequest<C> {
    pub send_time: Instant,
    pub request: RaftCmdRequest,
    pub ch: C,
}
```