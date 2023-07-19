#1.struct SnapManager

```rust

pub struct SnapManager {
    core: SnapManagerCore,
    max_total_size: Arc<AtomicU64>,

    // only used to receive snapshot from v2
    tablet_snap_manager: Option<TabletSnapManager>,
}

```