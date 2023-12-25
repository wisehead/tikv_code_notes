#1.struct Env

```rust
    pub(crate) inner: *mut DBEnv,
    #[allow(dead_code)]
    base: Option<Arc<Env>>,
}
```