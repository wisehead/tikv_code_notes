#1.struct ReadIndexRequest

```rust
pub struct ReadIndexRequest {
    // message fields
    pub start_ts: u64,
    pub key_ranges: ::protobuf::RepeatedField<super::kvrpcpb::KeyRange>,
    // special fields
    pub unknown_fields: ::protobuf::UnknownFields,
    pub cached_size: ::protobuf::CachedSize,
}
```