#1.trait StoreAddrResolver

```rust
/// A trait for resolving store addresses.
pub trait StoreAddrResolver: Send + Clone {
    /// Resolves the address for the specified store id asynchronously.
    fn resolve(&self, store_id: u64, cb: Callback) -> Result<()>;
}
```