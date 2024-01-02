#1.struct ExternalStorage

```rust
/// An abstraction of an external storage.
// TODO: these should all be returning a future (i.e. async fn).
#[async_trait]
pub trait ExternalStorage: 'static + Send + Sync {
    fn name(&self) -> &'static str;

    fn url(&self) -> io::Result<url::Url>;

    /// Write all contents of the read to the given path.
    async fn write(&self, name: &str, reader: UnpinReader, content_length: u64) -> io::Result<()>;

    /// Read all contents of the given path.
    fn read(&self, name: &str) -> ExternalData<'_>;

    /// Read part of contents of the given path.
    fn read_part(&self, name: &str, off: u64, len: u64) -> ExternalData<'_>;

    /// Read from external storage and restore to the given path
    async fn restore(
        &self,
        storage_name: &str,
        restore_name: std::path::PathBuf,
        expected_length: u64,
        speed_limiter: &Limiter,
        restore_config: RestoreConfig,
    ) -> io::Result<()> {
    }
}
```