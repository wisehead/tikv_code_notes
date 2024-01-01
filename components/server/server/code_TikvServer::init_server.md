#1.TikvServer::init_server

```
TikvServer::init_server
--let flow_controller = Arc::new(FlowController::Singleton(EngineFlowController::new(
            &self.core.config.storage.flow_control,
            self.engines.as_ref().unwrap().engine.kv_engine().unwrap(),
            self.core.flow_info_receiver.take().unwrap(),
        )));
--let mut gc_worker = self.init_gc_worker();
...
...
-- // Start backup stream
--let backup_stream_scheduler = if self.core.config.log_backup.enable {       
  // Create backup stream.
----let mut backup_stream_worker = Box::new(LazyWorker::new("backup-stream"));
----let backup_stream_scheduler = backup_stream_worker.scheduler();
    // Register backup-stream observer.
----let backup_stream_ob = BackupStreamObserver::new(backup_stream_scheduler.clone());
----backup_stream_ob.register_to(self.coprocessor_host.as_mut().unwrap());
----// Register config manager.
----cfg_controller.register(
                tikv::config::Module::BackupStream,
                Box::new(BackupStreamConfigManager::new(
                    backup_stream_worker.scheduler(),
                    self.core.config.log_backup.clone(),
                )),
            );
----let region_read_progress = engines
                .store_meta
                .lock()
                .unwrap()
                .region_read_progress
                .clone();
----let leadership_resolver = LeadershipResolver::new(
                node.id(),
                self.pd_client.clone(),
                self.env.clone(),
                self.security_mgr.clone(),
                region_read_progress,
                Duration::from_secs(60),
            );
----let backup_stream_endpoint = backup_stream::Endpoint::new(
                node.id(),
                PdStore::new(Checked::new(Sourced::new(
                    Arc::clone(&self.pd_client),
                    pd_client::meta_storage::Source::LogBackup,
                ))),
                self.core.config.log_backup.clone(),
                backup_stream_scheduler.clone(),
                backup_stream_ob,
                self.region_info_accessor.clone(),
                CdcRaftRouter(self.router.clone()),
                self.pd_client.clone(),
                self.concurrency_manager.clone(),
                BackupStreamResolver::V1(leadership_resolver),
            );            
```