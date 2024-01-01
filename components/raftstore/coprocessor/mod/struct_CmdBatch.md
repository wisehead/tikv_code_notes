#1.struct CmdBatch

```rust
pub struct CmdBatch {
    pub level: ObserveLevel,
    pub cdc_id: ObserveId,
    pub rts_id: ObserveId,
    pub pitr_id: ObserveId,
    pub region_id: u64,
    pub cmds: Vec<Cmd>,
}
```