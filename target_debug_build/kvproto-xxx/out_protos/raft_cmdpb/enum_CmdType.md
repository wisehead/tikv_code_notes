#1.enum CmdType

```rust
pub enum CmdType {
    Invalid = 0,
    Get = 1,
    Put = 3,
    Delete = 4,
    Snap = 5,
    Prewrite = 6,
    DeleteRange = 7,
    IngestSst = 8,
    ReadIndex = 9,
}
```