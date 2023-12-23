#1.message Peer

```rust
message Peer {
    uint64 id = 1;
    uint64 store_id = 2;
    PeerRole role = 3;
    bool is_witness = 4;
}
```