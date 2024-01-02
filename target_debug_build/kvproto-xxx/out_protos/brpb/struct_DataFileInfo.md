#1.struct DataFileInfo

```rust
pub struct DataFileInfo {
    // message fields
    pub sha256: ::std::vec::Vec<u8>,
    pub path: ::std::string::String,
    pub number_of_entries: i64,
    pub min_ts: u64,
    pub max_ts: u64,
    pub resolved_ts: u64,
    pub region_id: i64,
    pub start_key: ::std::vec::Vec<u8>,
    pub end_key: ::std::vec::Vec<u8>,
    pub cf: ::std::string::String,
    pub r_type: FileType,
    pub is_meta: bool,
    pub table_id: i64,
    pub length: u64,
    pub min_begin_ts_in_default_cf: u64,
    pub range_offset: u64,
    pub range_length: u64,
    pub compression_type: CompressionType,
    // special fields
    pub unknown_fields: ::protobuf::UnknownFields,
    pub cached_size: ::protobuf::CachedSize,
}
```