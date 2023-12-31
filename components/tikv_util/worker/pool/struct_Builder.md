#1.struct Builder

```rust
pub struct Builder<S: Into<String>> {
    name: S,
    thread_count: usize,
    pending_capacity: usize,
}
```