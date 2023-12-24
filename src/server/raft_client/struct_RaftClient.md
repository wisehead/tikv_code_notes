#1.struct RaftClient

```rust
/// A raft client that can manages connections correctly.
///
/// A correct usage of raft client is:
///
/// ```text
/// for m in msgs {
///     if !raft_client.send(m) {
///         // handle error.
///     }
/// }
/// raft_client.flush();
/// ```
pub struct RaftClient<S, R> {
    self_store_id: u64,
    pool: Arc<Mutex<ConnectionPool>>,
    cache: LruCache<(u64, usize), CachedQueue>,
    need_flush: Vec<(u64, usize)>,
    full_stores: Vec<(u64, usize)>,
    future_pool: Arc<ThreadPool<TaskCell>>,
    builder: ConnectionBuilder<S, R>,
    last_hash: (u64, u64),
}
```