#1.struct DataFileGroup

```rust
pub struct DataFileGroup {
    // message fields
    pub path: ::std::string::String,
    pub data_files_info: ::protobuf::RepeatedField<DataFileInfo>,
    pub min_ts: u64,
    pub max_ts: u64,
    pub min_resolved_ts: u64,
    pub length: u64,
    // special fields
    pub unknown_fields: ::protobuf::UnknownFields,
    pub cached_size: ::protobuf::CachedSize,
}
```