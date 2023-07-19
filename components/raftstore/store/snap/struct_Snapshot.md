#1.struct Snapshot

```rust
pub struct Snapshot {
    key: SnapKey,
    display_path: String,
    dir_path: PathBuf,
    cf_files: Vec<CfFile>,
    cf_index: usize,
    cf_file_index: usize,
    meta_file: MetaFile,
    hold_tmp_files: bool,

    mgr: SnapManagerCore,
}


```