#1.struct ManagedFileSystem

```rust
pub struct ManagedFileSystem {
    base_file_system: DefaultFileSystem,
    key_manager: Option<Arc<DataKeyManager>>,
    rate_limiter: Option<Arc<IoRateLimiter>>,
}
```