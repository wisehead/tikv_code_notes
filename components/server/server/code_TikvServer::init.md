#1.TikvServer::init

```
TikvServer::init
--let env = Arc::new(
            EnvBuilder::new()
                .cq_count(config.server.grpc_concurrency)
                .name_prefix(thd_name!(GRPC_THREAD_PREFIX))
                .after_start(|| {
                    // SAFETY: we will call `remove_thread_memory_accessor` at before_stop.
                    unsafe { add_thread_memory_accessor() };
                })
                .before_stop(|| {
                    remove_thread_memory_accessor();
                })
                .build(),
        );
--let pd_client = TikvServerCore::connect_to_pd_cluster(
            &mut config,
            env.clone(),
            Arc::clone(&security_mgr),
        );
--let is_recovering_marked = match pd_client.is_recovering_marked()
--if is_recovering_marked {
----snap_recovery::init_cluster::enter_snap_recovery_mode(&mut config);
----let cluster_id = config.server.cluster_id;
----snap_recovery::init_cluster::start_recovery(
                config.clone(),
                cluster_id,
                pd_client.clone(),
            );        
--let cfg_controller = TikvServerCore::init_config(config);
--let background_worker = WorkerBuilder::new("background")
            .thread_count(thread_count)
            .create();        
--let (router, system) = fsm::create_raft_batch_system(&config.raft_store, &resource_manager);
```