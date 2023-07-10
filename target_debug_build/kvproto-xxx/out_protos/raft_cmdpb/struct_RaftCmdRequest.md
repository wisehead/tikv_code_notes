#1.struct RaftCmdRequest

```rust
pub struct RaftCmdRequest {
    // message fields
    pub header: ::protobuf::SingularPtrField<RaftRequestHeader>,
    pub requests: ::protobuf::RepeatedField<Request>,
    pub admin_request: ::protobuf::SingularPtrField<AdminRequest>,
    pub status_request: ::protobuf::SingularPtrField<StatusRequest>,
    // special fields
    pub unknown_fields: ::protobuf::UnknownFields,
    pub cached_size: ::protobuf::CachedSize,
}
```