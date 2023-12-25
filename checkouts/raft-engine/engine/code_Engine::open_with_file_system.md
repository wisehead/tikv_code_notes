#1.Engine::open_with_file_system

```
Engine::open_with_file_system
--let mut builder = FilePipeLogBuilder::new(cfg.clone(), file_system, listeners.clone());
--builder.scan()?;
--let factory = MemTableRecoverContextFactory::new(&cfg);
--let (append, rewrite) = builder.recover(&factory)?;
--rewrite.merge_append_context(append);
--let (memtables, stats) = rewrite.finish();
--let purge_manager = PurgeManager::new(
            cfg.clone(),
            memtables.clone(),
            pipe_log.clone(),
            stats.clone(),
            listeners.clone(),
        );
--let (tx, rx) = mpsc::channel();
        let stats_clone = stats.clone();
        let memtables_clone = memtables.clone();
        let metrics_flusher = ThreadBuilder::new()
            .name("re-metrics".into())
            .spawn(move || loop {
                stats_clone.flush_metrics();
                memtables_clone.flush_metrics();
                if rx.recv_timeout(METRICS_FLUSH_INTERVAL).is_ok() {
                    break;
                }
            })?;        
```