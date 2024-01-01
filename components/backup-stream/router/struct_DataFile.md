#1.struct DataFile

```rust
/// A opened log file with some metadata.
struct DataFile {
    min_ts: TimeStamp,
    max_ts: TimeStamp,
    resolved_ts: TimeStamp,
    min_begin_ts: Option<TimeStamp>,
    sha256: Hasher,
    // TODO: use lz4 with async feature
    inner: tempfiles::ForWrite,
    compression_type: CompressionType,
    start_key: Vec<u8>,
    end_key: Vec<u8>,
    number_of_entries: usize,
    file_size: usize,
}
```