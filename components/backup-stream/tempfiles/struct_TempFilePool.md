#1.struct TempFilePool

```rust
pub struct TempFilePool {
    cfg: Config,
    current: AtomicUsize,
    files: BlockMutex<FileSet>,

    #[cfg(test)]
    override_swapout: Option<
        Box<dyn Fn(&Path) -> Pin<Box<dyn AsyncWrite + Send + 'static>> + Send + Sync + 'static>,
    >,
}
```