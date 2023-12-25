#1.struct TikvServerCore

```rust
/// This is the common part of TiKV-like servers. It is a collection of all
/// capabilities a TikvServer should have or may take advantage of. By holding
/// it in its own TikvServer implementation, one can easily access the common
/// ability of a TiKV server.
// Fields in this struct are all public since they are open for other TikvServer
// to use, e.g. a custom TikvServer may alter some fields in `config` or push
// some services into `to_stop`.
pub struct TikvServerCore {
    pub config: TikvConfig,
    pub store_path: PathBuf,
    pub lock_files: Vec<File>,
    pub encryption_key_manager: Option<Arc<DataKeyManager>>,
    pub flow_info_sender: Option<mpsc::Sender<FlowInfo>>,
    pub flow_info_receiver: Option<mpsc::Receiver<FlowInfo>>,
    pub to_stop: Vec<Box<dyn Stop>>,
    pub background_worker: Worker,
}
```