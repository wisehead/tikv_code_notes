#1.TikvServer::init_raw_engines

```
TikvServer::init_raw_engines
--let (raft_engine, raft_statistics) = CER::build(
            &self.core.config,
            &env,
            &self.core.encryption_key_manager,
            &block_cache,
        );
--let builder = KvEngineFactoryBuilder::new(
            env,
            &self.core.config,
            block_cache,
            self.core.encryption_key_manager.clone(),
        )
        .compaction_event_sender(Arc::new(RaftRouterCompactedEventSender {
            router: Mutex::new(self.router.clone()),
        }))
        .region_info_accessor(self.region_info_accessor.clone())
        .sst_recovery_sender(self.init_sst_recovery_sender())
        .flow_listener(flow_listener);
--let factory = Box::new(builder.build());
--let kv_engine = factory
            .create_shared_db(&self.core.store_path)
            .unwrap_or_else(|s| fatal!("failed to create kv engine: {}", s));
--self.kv_statistics = Some(factory.rocks_statistics());
--let engines = Engines::new(kv_engine.clone(), raft_engine);        
--
```