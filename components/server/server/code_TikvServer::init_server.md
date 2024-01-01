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
----
```