#1.create_local_engine_service

```
create_local_engine_service
--let key_manager =
        data_key_manager_from_config(&config.security.encryption, &config.storage.data_dir)
--let env = config
        .build_shared_rocks_env(key_manager.clone(), None)
--let block_cache = config.storage.block_cache.build_shared_cache();
--let factory =
        KvEngineFactoryBuilder::new(env.clone(), config, block_cache, key_manager.clone())
            .lite(true)
            .build();
--let kv_db = match factory.create_shared_db(&config.storage.data_dir) 
--if !config.raft_engine.enable {
----raft_db = match new_engine_opt(&raft_path, raft_db_opts, raft_db_cf_opts)
----let local_engines = LocalEngines::new(Engines::new(kv_db, raft_db));
--else
----let raft_db = RaftLogEngine::new(cfg, key_manager, None /* io_rate_limiter */).unwrap();
----let local_engines = LocalEngines::new(Engines::new(kv_db, raft_db));
```