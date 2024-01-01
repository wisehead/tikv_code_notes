#1.struct WorkerThread

```rust
pub(crate) struct WorkerThread<T, R> {
    local: Local<T>,
    runner: R,
}
```