#1.struct Engine

```rust
pub struct Engine<F = DefaultFileSystem, P = FilePipeLog<F>>
where
    F: FileSystem,
    P: PipeLog,
{
    cfg: Arc<Config>,
    listeners: Vec<Arc<dyn EventListener>>,

    #[allow(dead_code)]
    stats: Arc<GlobalStats>,
    memtables: MemTables,
    pipe_log: Arc<P>,
    purge_manager: PurgeManager<P>,

    write_barrier: WriteBarrier<LogBatch, Result<FileBlockHandle>>,

    tx: Mutex<mpsc::Sender<()>>,
    metrics_flusher: Option<JoinHandle<()>>,

    _phantom: PhantomData<F>,
}
```