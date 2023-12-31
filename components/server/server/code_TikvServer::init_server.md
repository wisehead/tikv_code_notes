#1.TikvServer::init_server

```
TikvServer::init_server
--let flow_controller = Arc::new(FlowController::Singleton(EngineFlowController::new(
            &self.core.config.storage.flow_control,
            self.engines.as_ref().unwrap().engine.kv_engine().unwrap(),
            self.core.flow_info_receiver.take().unwrap(),
        )));
--        
```