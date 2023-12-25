#1.struct FlowListener

```rust
pub struct FlowListener {
    flow_info_sender: Arc<Mutex<Sender<FlowInfo>>>,
    region_id: u64,
}

```