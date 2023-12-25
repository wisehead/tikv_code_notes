#1.RaftLogEngine::new

```
RaftLogEngine::new
--let file_system = Arc::new(ManagedFileSystem::new(key_manager, rate_limiter));
--Ok(RaftLogEngine(Arc::new(
            RawRaftEngine::open_with_file_system(config, file_system).map_err(transfer_error)?,
        )))
```