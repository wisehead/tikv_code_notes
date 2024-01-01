#1.struct ConfigInner

```rust
struct ConfigInner {
    current: TikvConfig,
    config_mgrs: HashMap<Module, Box<dyn ConfigManager>>,
}
```