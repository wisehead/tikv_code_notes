#1.struct StoreFsm

```rust
pub struct StoreFsm {
    store: Store,
    receiver: Receiver<StoreMsg>,
}
```