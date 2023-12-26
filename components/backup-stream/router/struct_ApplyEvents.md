#1.struct ApplyEvents

```rust
pub struct ApplyEvents {
    events: Vec<ApplyEvent>,
    region_id: u64,
    // TODO: this field is useless, maybe remove it.
    region_resolved_ts: u64,
}
```