#1.struct TempFileKey

```rust
/// The handle of a temporary file.
#[derive(Debug, PartialEq, Eq, Clone, Copy, Hash)]
struct TempFileKey {
    table_id: i64,
    region_id: u64,
    cf: CfName,
    cmd_type: CmdType,
    is_meta: bool,
}
```