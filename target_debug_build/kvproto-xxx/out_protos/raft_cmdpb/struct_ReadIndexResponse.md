#1.struct ReadIndexResponse

```rust
pub struct ReadIndexResponse {
    // message fields
    pub read_index: u64,
    pub locked: ::protobuf::SingularPtrField<super::kvrpcpb::LockInfo>,
    // special fields
    pub unknown_fields: ::protobuf::UnknownFields,
    pub cached_size: ::protobuf::CachedSize,
}
```