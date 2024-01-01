#1.struct Registry

```rust
/// Registry contains all registered coprocessors.
#[derive(Clone)]
pub struct Registry<E>
where
    E: KvEngine + 'static,
{
    admin_observers: Vec<Entry<BoxAdminObserver>>,
    query_observers: Vec<Entry<BoxQueryObserver>>,
    apply_snapshot_observers: Vec<Entry<BoxApplySnapshotObserver>>,
    split_check_observers: Vec<Entry<BoxSplitCheckObserver<E>>>,
    consistency_check_observers: Vec<Entry<BoxConsistencyCheckObserver<E>>>,
    role_observers: Vec<Entry<BoxRoleObserver>>,
    region_change_observers: Vec<Entry<BoxRegionChangeObserver>>,
    cmd_observers: Vec<Entry<BoxCmdObserver<E>>>,
    read_index_observers: Vec<Entry<BoxReadIndexObserver>>,
    pd_task_observers: Vec<Entry<BoxPdTaskObserver>>,
    update_safe_ts_observers: Vec<Entry<BoxUpdateSafeTsObserver>>,
    message_observers: Vec<Entry<BoxMessageObserver>>,
    // TODO: add endpoint
}
```