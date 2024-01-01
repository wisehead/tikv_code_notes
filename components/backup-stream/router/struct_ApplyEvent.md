#1.struct ApplyEvent

```rust
pub struct ApplyEvent {
    pub key: Vec<u8>,
    pub value: Vec<u8>,
    pub cf: CfName,
    pub cmd_type: CmdType,
}
```