#1.enum Task

```rust
pub enum Task {
    PeerStorage {
        region_id: u64,
        context: GetEntriesContext,
        low: u64,
        high: u64,
        max_size: usize,
        tried_cnt: usize,
        term: u64,
    },
    // More to support, suck as fetch entries ayschronously when apply and schedule merge
}

```