#1.struct PdStoreAddrResolver

```rust
/// A store address resolver which is backed by a `PDClient`.
#[derive(Clone)]
pub struct PdStoreAddrResolver {
    sched: Scheduler<Task>,
}
```