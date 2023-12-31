#1.struct StoreMeta

```rust
pub struct StoreMeta {
    pub store_id: Option<u64>,
    /// region_end_key -> region_id
    pub region_ranges: BTreeMap<Vec<u8>, u64>,
    /// region_id -> region
    pub regions: HashMap<u64, Region>,
    /// region_id -> reader
    pub readers: HashMap<u64, ReadDelegate>,
    /// `MsgRequestPreVote`, `MsgRequestVote` or `MsgAppend` messages from newly
    /// split Regions shouldn't be dropped if there is no such Region in this
    /// store now. So the messages are recorded temporarily and will be handled
    /// later.
    pub pending_msgs: RingQueue<RaftMessage>,
    /// The regions with pending snapshots.
    pub pending_snapshot_regions: Vec<Region>,
    /// A marker used to indicate the peer of a Region has received a merge
    /// target message and waits to be destroyed. target_region_id ->
    /// (source_region_id -> merge_target_region)
    pub pending_merge_targets: HashMap<u64, HashMap<u64, metapb::Region>>,
    /// An inverse mapping of `pending_merge_targets` used to let source peer
    /// help target peer to clean up related entry. source_region_id ->
    /// target_region_id
    pub targets_map: HashMap<u64, u64>,
    /// `atomic_snap_regions` and `destroyed_region_for_snap` are used for
    /// making destroy overlapped regions and apply snapshot atomically.
    /// region_id -> wait_destroy_regions_map(source_region_id -> is_ready)
    /// A target peer must wait for all source peer to ready before applying
    /// snapshot.
    pub atomic_snap_regions: HashMap<u64, HashMap<u64, bool>>,
    /// source_region_id -> need_atomic
    /// Used for reminding the source peer to switch to ready in
    /// `atomic_snap_regions`.
    pub destroyed_region_for_snap: HashMap<u64, bool>,
    /// region_id -> `RegionReadProgress`
    pub region_read_progress: RegionReadProgressRegistry,
    /// record sst_file_name -> (sst_smallest_key, sst_largest_key)
    pub damaged_ranges: HashMap<String, (Vec<u8>, Vec<u8>)>,
}


```