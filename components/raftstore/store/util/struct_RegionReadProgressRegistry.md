#1.struct RegionReadProgressRegistry

```rust
pub struct RegionReadProgressRegistry {
    registry: Arc<Mutex<HashMap<u64, Arc<RegionReadProgress>>>>,
}
```