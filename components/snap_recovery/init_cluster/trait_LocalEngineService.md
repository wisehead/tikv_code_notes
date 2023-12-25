#1.trait LocalEngineService

```rust
// the service to operator the local engines
pub trait LocalEngineService {
    fn set_cluster_id(&self, cluster_id: u64);
    fn get_store_id(&self) -> Result<u64>;
}
```