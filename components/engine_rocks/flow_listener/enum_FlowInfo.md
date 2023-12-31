#1.enum FlowInfo

```rust
pub enum FlowInfo {
    L0(String, u64, u64),
    L0Intra(String, u64, u64),
    Flush(String, u64, u64),
    Compaction(String, u64),
    BeforeUnsafeDestroyRange(u64),
    AfterUnsafeDestroyRange(u64),
    Created(u64),
    Destroyed(u64),
}
```