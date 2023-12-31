#1.trait Engine

```rust
/// Engine defines the common behaviour for a storage engine type.
pub trait Engine: Send + Clone + 'static {
    type Snap: Snapshot;
    type Local: LocalEngine;

    /// Local storage engine.
    ///
    /// If local engine can't be accessed directly, `None` is returned.
    /// Currently, only multi-rocksdb version will return `None`.
    fn kv_engine(&self) -> Option<Self::Local>;

    type RaftExtension: raft_extension::RaftExtension = FakeExtension;
    /// Get the underlying raft extension.
    fn raft_extension(&self) -> Self::RaftExtension {
        unimplemented!()
    }

    /// Write modifications into internal local engine directly.
    ///
    /// region_modifies records each region's modifications.
    fn modify_on_kv_engine(&self, region_modifies: HashMap<u64, Vec<Modify>>) -> Result<()>;

    type SnapshotRes: Future<Output = Result<Self::Snap>> + Send + 'static;
    /// Get a snapshot asynchronously.
    ///
    /// Note the snapshot is queried immediately no matter whether the returned
    /// future is polled or not.
    fn async_snapshot(&mut self, ctx: SnapContext<'_>) -> Self::SnapshotRes;

    /// Precheck request which has write with it's context.
    fn precheck_write_with_ctx(&self, _ctx: &Context) -> Result<()> {
        Ok(())
    }

    type WriteRes: Stream<Item = WriteEvent> + Unpin + Send + 'static;
    /// Writes data to the engine asynchronously.
    ///
    /// You can subscribe special events like `EVENT_PROPOSED` and
    /// `EVENT_COMMITTED`.
    ///
    /// `on_applied` is called right in the processing thread before being
    /// fed to the stream.
    ///
    /// Note the write is started no matter whether the returned stream is
    /// polled or not.
    fn async_write(
        &self,
        ctx: &Context,
        batch: WriteData,
        subscribed: u8,
        on_applied: Option<OnAppliedCb>,
    ) -> Self::WriteRes;

    fn write(&self, ctx: &Context, batch: WriteData) -> Result<()> {
        let f = write(self, ctx, batch, None);
        let res = block_on_timeout(f, DEFAULT_TIMEOUT)
            .map_err(|_| Error::from(ErrorInner::Timeout(DEFAULT_TIMEOUT)))?;
        if let Some(res) = res {
            return res;
        }
        Err(Error::from(ErrorInner::Timeout(DEFAULT_TIMEOUT)))
    }

    fn release_snapshot(&mut self) {}

    fn snapshot(&mut self, ctx: SnapContext<'_>) -> Result<Self::Snap> {
        block_on_timeout(self.async_snapshot(ctx), DEFAULT_TIMEOUT)
            .map_err(|_| Error::from(ErrorInner::Timeout(DEFAULT_TIMEOUT)))?
    }

    fn put(&self, ctx: &Context, key: Key, value: Value) -> Result<()> {
        self.put_cf(ctx, CF_DEFAULT, key, value)
    }

    fn put_cf(&self, ctx: &Context, cf: CfName, key: Key, value: Value) -> Result<()> {
        self.write(
            ctx,
            WriteData::from_modifies(vec![Modify::Put(cf, key, value)]),
        )
    }

    fn delete(&self, ctx: &Context, key: Key) -> Result<()> {
        self.delete_cf(ctx, CF_DEFAULT, key)
    }

    fn delete_cf(&self, ctx: &Context, cf: CfName, key: Key) -> Result<()> {
        self.write(ctx, WriteData::from_modifies(vec![Modify::Delete(cf, key)]))
    }

    fn get_mvcc_properties_cf(
        &self,
        _: CfName,
        _safe_point: TimeStamp,
        _start: &[u8],
        _end: &[u8],
    ) -> Option<MvccProperties> {
        None
    }

    // Some engines have a `TxnExtraScheduler`. This method is to send the extra
    // to the scheduler.
    fn schedule_txn_extra(&self, _txn_extra: TxnExtra) {}

    /// Mark the start of flashback.
    // It's an infrequent API, use trait object for simplicity.
    fn start_flashback(&self, _ctx: &Context, _start_ts: u64) -> BoxFuture<'static, Result<()>> {
        Box::pin(futures::future::ready(Ok(())))
    }

    /// Mark the end of flashback.
    // It's an infrequent API, use trait object for simplicity.
    fn end_flashback(&self, _ctx: &Context) -> BoxFuture<'static, Result<()>> {
        Box::pin(futures::future::ready(Ok(())))
    }

    /// Application may operate on local engine directly, the method is to hint
    /// the engine there is probably a notable difference in range, so
    /// engine may update its statistics.
    fn hint_change_in_range(&self, _start_key: Vec<u8>, _end_key: Vec<u8>) {}
}
```