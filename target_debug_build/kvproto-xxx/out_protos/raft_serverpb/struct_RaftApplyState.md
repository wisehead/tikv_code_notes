#1.struct RaftApplyState

```rust
pub struct RaftApplyState {
    // message fields
    pub applied_index: u64,
    pub last_commit_index: u64,
    pub commit_index: u64,
    pub commit_term: u64,
    pub truncated_state: ::protobuf::SingularPtrField<RaftTruncatedState>,
    // special fields
    pub unknown_fields: ::protobuf::UnknownFields,
    pub cached_size: ::protobuf::CachedSize,
}

```