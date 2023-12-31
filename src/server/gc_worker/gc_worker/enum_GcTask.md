#1.enum GcTask
```rust
pub enum GcTask<E>
where
    E: KvEngine,
{
    Gc {
        region: Region,
        safe_point: TimeStamp,
        callback: Callback<()>,
    },
    GcKeys {
        keys: Vec<Key>,
        safe_point: TimeStamp,
        region_info_provider: Arc<dyn RegionInfoProvider>,
    },
    RawGcKeys {
        keys: Vec<Key>,
        safe_point: TimeStamp,
        region_info_provider: Arc<dyn RegionInfoProvider>,
    },
    UnsafeDestroyRange {
        ctx: Context,
        start_key: Key,
        end_key: Key,
        callback: Callback<()>,
        region_info_provider: Arc<dyn RegionInfoProvider>,
    },
    /// If GC in compaction filter is enabled, versions on default CF will be
    /// handled with `DB::delete` in write CF's compaction filter. However if
    /// the compaction filter finds the DB is stalled, it will send the task
    /// to GC worker to ensure the compaction can be continued.
    ///
    /// NOTE: It's possible that the TiKV instance fails after a compaction
    /// result is installed but its orphan versions are not deleted. Those
    /// orphan versions will never get cleaned
    /// until `DefaultCompactionFilter` is introduced.
    ///
    /// The tracking issue: <https://github.com/tikv/tikv/issues/9719>.
    OrphanVersions {
        wb: DeleteBatch<E::WriteBatch>,
        id: usize,
        region_info_provider: Arc<dyn RegionInfoProvider>,
    },
    #[cfg(any(test, feature = "testexport"))]
    Validate(Box<dyn FnOnce(&GcConfig, &Limiter) + Send>),
}

```