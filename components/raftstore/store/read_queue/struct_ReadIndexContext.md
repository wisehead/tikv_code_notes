#1.struct ReadIndexContext

```rust
pub struct ReadIndexContext {
    pub id: Uuid,
    pub request: Option<raft_cmdpb::ReadIndexRequest>,
    pub locked: Option<LockInfo>,
}
```