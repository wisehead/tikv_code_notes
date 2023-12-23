#1.struct ReadDelegate

```rust
/// #[RaftstoreCommon]: A read only delegate of `Peer`.
#[derive(Clone, Debug)]
pub struct ReadDelegate {
    pub region: Arc<metapb::Region>,
    pub peer_id: u64,
    pub term: u64,
    pub applied_term: u64,
    pub leader_lease: Option<RemoteLease>,
    pub last_valid_ts: Timespec,

    pub tag: String,
    pub bucket_meta: Option<Arc<BucketMeta>>,
    pub txn_extra_op: Arc<AtomicCell<TxnExtraOp>>,
    pub txn_ext: Arc<TxnExt>,
    pub read_progress: Arc<RegionReadProgress>,
    pub pending_remove: bool,

    // `track_ver` used to keep the local `ReadDelegate` in `LocalReader`
    // up-to-date with the global `ReadDelegate` stored at `StoreMeta`
    pub track_ver: TrackVer,
}
```