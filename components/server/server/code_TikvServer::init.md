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
--let mut coprocessor_host = Some(CoprocessorHost::new(
            router.clone(),
            config.coprocessor.clone(),
        ));

--let region_info_accessor = RegionInfoAccessor::new(coprocessor_host.as_mut().unwrap());

        // Initialize concurrency manager
--let latest_ts = block_on(pd_client.get_tso()).expect("failed to get timestamp from PD");
--let concurrency_manager = ConcurrencyManager::new(latest_ts);
--let quota_limiter = Arc::new(QuotaLimiter::new(
            config.quota.foreground_cpu_time,
            config.quota.foreground_write_bandwidth,
            config.quota.foreground_read_bandwidth,
            config.quota.background_cpu_time,
            config.quota.background_write_bandwidth,
            config.quota.background_read_bandwidth,
            config.quota.max_delay_duration,
            config.quota.enable_auto_tune,
        ));
--let check_leader_worker = WorkerBuilder::new("check-leader").thread_count(1).create();
--TikvServer {
            core: TikvServerCore {
                config,
                store_path,
                lock_files: vec![],
                encryption_key_manager: None,
                flow_info_sender: None,
                flow_info_receiver: None,
                to_stop: vec![],
                background_worker,
            },
            cfg_controller: Some(cfg_controller),
            security_mgr,
            pd_client,
            router,
            system: Some(system),
            resolver: None,
            snap_mgr: None,
            engines: None,
            kv_statistics: None,
            raft_statistics: None,
            servers: None,
            region_info_accessor,
            coprocessor_host,
            concurrency_manager,
            env,
            check_leader_worker,
            sst_worker: None,
            quota_limiter,
            resource_manager,
            causal_ts_provider,
            tablet_registry: None,
            br_snap_recovery_mode: is_recovering_marked,
            resolved_ts_scheduler: None,
            grpc_service_mgr: GrpcServiceManager::new(tx),
        }        
```