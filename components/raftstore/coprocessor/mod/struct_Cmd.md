#1.struct Cmd

```rust
pub struct Cmd {
    pub index: u64,
    pub term: u64,
    pub request: RaftCmdRequest,
    pub response: RaftCmdResponse,
}
```