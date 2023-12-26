#1.trait RaftEngine

```rust
pub trait RaftEngine: RaftEngineReadOnly + PerfContextExt + Clone + Sync + Send + 'static {
    type LogBatch: RaftLogBatch;

    fn log_batch(&self, capacity: usize) -> Self::LogBatch;

    /// Synchronize the Raft engine.
    fn sync(&self) -> Result<()>;

    /// Consume the write batch by moving the content into the engine itself
    /// and return written bytes.
    fn consume(&self, batch: &mut Self::LogBatch, sync: bool) -> Result<usize>;

    /// Like `consume` but shrink `batch` if need.
    fn consume_and_shrink(
        &self,
        batch: &mut Self::LogBatch,
        sync: bool,
        max_capacity: usize,
        shrink_to: usize,
    ) -> Result<usize>;

    fn clean(
        &self,
        raft_group_id: u64,
        first_index: u64,
        state: &RaftLocalState,
        batch: &mut Self::LogBatch,
    ) -> Result<()>;

    /// Like `cut_logs` but the range could be very large.
    fn gc(&self, raft_group_id: u64, from: u64, to: u64, batch: &mut Self::LogBatch) -> Result<()>;

    /// Delete all but the latest one of states that are associated with smaller
    /// apply_index.
    fn delete_all_but_one_states_before(
        &self,
        raft_group_id: u64,
        apply_index: u64,
        batch: &mut Self::LogBatch,
    ) -> Result<()>;

    fn need_manual_purge(&self) -> bool {
        false
    }

    /// Purge expired logs files and return a set of Raft group ids
    /// which needs to be compacted ASAP.
    fn manual_purge(&self) -> Result<Vec<u64>> {
        unimplemented!()
    }

    fn flush_metrics(&self, _instance: &str) {}
    fn flush_stats(&self) -> Option<CacheStats> {
        None
    }

    fn stop(&self) {}

    fn dump_stats(&self) -> Result<String>;

    fn get_engine_size(&self) -> Result<u64>;

    /// The path to the directory on the filesystem where the raft log is stored
    fn get_engine_path(&self) -> &str;

    /// Visit all available raft groups.
    ///
    /// If any error is returned, the iteration will stop.
    fn for_each_raft_group<E, F>(&self, f: &mut F) -> std::result::Result<(), E>
    where
        F: FnMut(u64) -> std::result::Result<(), E>,
        E: From<Error>;
}
```