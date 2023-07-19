#1.trait ApplySnapshotObserver

```rust
pub trait ApplySnapshotObserver: Coprocessor {
    /// Hook to call after applying key from plain file.
    /// This may be invoked multiple times for each plain file, and each time a
    /// batch of key-value pairs will be passed to the function.
    fn apply_plain_kvs(&self, _: &mut ObserverContext<'_>, _: CfName, _: &[(Vec<u8>, Vec<u8>)]) {}

    /// Hook to call after applying sst file. Currently the content of the
    /// snapshot can't be passed to the observer.
    fn apply_sst(&self, _: &mut ObserverContext<'_>, _: CfName, _path: &str) {}

    /// Hook when receiving Task::Apply.
    /// Should pass valid snapshot, the option is only for testing.
    /// Notice that we can call `pre_apply_snapshot` to multiple snapshots at
    /// the same time.
    fn pre_apply_snapshot(
        &self,
        _: &mut ObserverContext<'_>,
        _peer_id: u64,
        _: &crate::store::SnapKey,
        _: Option<&crate::store::Snapshot>,
    ) {
    }

    /// Hook when the whole snapshot is applied.
    /// Should pass valid snapshot, the option is only for testing.
    fn post_apply_snapshot(
        &self,
        _: &mut ObserverContext<'_>,
        _: u64,
        _: &crate::store::SnapKey,
        _snapshot: Option<&crate::store::Snapshot>,
    ) {
    }

    /// We call pre_apply_snapshot only when one of the observer returns true.
    fn should_pre_apply_snapshot(&self) -> bool {
        false
    }
}
```