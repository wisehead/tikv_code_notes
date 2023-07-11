#1.struct PutRequest

```rust
pub struct PutRequest {
    // message fields
    pub cf: ::std::string::String,
    pub key: ::std::vec::Vec<u8>,
    pub value: ::std::vec::Vec<u8>,
    // special fields
    pub unknown_fields: ::protobuf::UnknownFields,
    pub cached_size: ::protobuf::CachedSize,
}
```