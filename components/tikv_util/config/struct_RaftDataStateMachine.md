#1.struct RaftDataStateMachine

```rust
/// Helper for migrating Raft data safely. Such migration is defined as
/// multiple states that can be uniquely distinguished. And the transitions
/// between these states are atomic.
///
/// States:
///   1. Init - Only source directory contains Raft data.
///   2. Migrating - A marker file contains the path of source directory. The
/// source      directory contains a complete copy of Raft data. Target
/// directory may exist.   3. Completed - Only target directory contains Raft
/// data. Marker file may exist.
pub struct RaftDataStateMachine {
    root: PathBuf,
    in_progress_marker: PathBuf,
    source: PathBuf,
    target: PathBuf,
}
```