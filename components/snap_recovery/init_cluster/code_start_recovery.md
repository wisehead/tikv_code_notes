#1.start_recovery

```
start_recovery
--let local_engine_service = create_local_engine_service(&config)
--local_engine_service.set_cluster_id(cluster_id);
--let store_id = local_engine_service.get_store_id()
--let server_config = Arc::new(VersionTrack::new(config.server.clone()));
--let _ = bootcluster(
        &server_config.value().clone(),
        cluster_id,
        store_id,
        pd_client,
    );
```