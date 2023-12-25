#1.create_local_engine_service

```
create_local_engine_service
--let key_manager =
        data_key_manager_from_config(&config.security.encryption, &config.storage.data_dir)
--let env = config
        .build_shared_rocks_env(key_manager.clone(), None)
--let block_cache = config.storage.block_cache.build_shared_cache();
```