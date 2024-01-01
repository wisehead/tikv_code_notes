#1.trait ConfigManager

```rust
pub trait ConfigManager: Send + Sync {
    fn dispatch(&mut self, _: ConfigChange) -> Result<()>;
}
```