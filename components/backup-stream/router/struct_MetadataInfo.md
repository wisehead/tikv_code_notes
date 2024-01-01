#1.struct MetadataInfo

```rust
pub struct MetadataInfo {
    // the field files is deprecated in v6.3.0
    // pub files: Vec<DataFileInfo>,
    pub file_groups: Vec<DataFileGroup>,
    pub min_resolved_ts: Option<u64>,
    pub min_ts: Option<u64>,
    pub max_ts: Option<u64>,
    pub store_id: u64,
}
```