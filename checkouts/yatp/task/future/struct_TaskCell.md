#1.struct TaskCell

```rust
/// Wrapper of `RawTask` for easy use.
pub struct TaskCell(NonNull<RawTask<()>>);
```