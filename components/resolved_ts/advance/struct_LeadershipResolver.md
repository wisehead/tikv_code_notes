#1.struct LeadershipResolver

```rust
pub struct LeadershipResolver {
    tikv_clients: Mutex<HashMap<u64, TikvClient>>,
    pd_client: Arc<dyn PdClient>,
    env: Arc<Environment>,
    security_mgr: Arc<SecurityManager>,
    region_read_progress: RegionReadProgressRegistry,
    store_id: u64,

    // store_id -> check leader request, record the request to each stores.
    store_req_map: HashMap<u64, CheckLeaderRequest>,
    progresses: HashMap<u64, RegionProgress>,
    checking_regions: HashSet<u64>,
    valid_regions: HashSet<u64>,

    gc_interval: Duration,
    last_gc_time: Instant,
}
```