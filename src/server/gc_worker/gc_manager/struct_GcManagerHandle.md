#1.struct GcManagerHandle

```rust
/// Wraps `JoinHandle` of `GcManager` and helps to stop the `GcManager`
/// synchronously.
pub(super) struct GcManagerHandle {
    join_handle: JoinHandle<()>,
    stop_signal_sender: mpsc::Sender<()>,
}
```