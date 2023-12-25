#1.create_system

```
create_system
--let normal_scheduler = NormalScheduler {
        sender: sender.clone(),
        low_sender,
    };
    let control_scheduler = ControlScheduler {
        sender: sender.clone(),
    };
    let pool_state_builder = PoolStateBuilder {
        max_batch_size: cfg.max_batch_size(),
        reschedule_duration: cfg.reschedule_duration.0,
        fsm_receiver: receiver.clone(),
        fsm_sender: sender,
        pool_size: cfg.pool_size,
    };
    let router = Router::new(control_box, normal_scheduler, control_scheduler, state_cnt);
    let system = BatchSystem {
        name_prefix: None,
        router: router.clone(),
        receiver,
        low_receiver,
        pool_size: cfg.pool_size,
        max_batch_size: cfg.max_batch_size(),
        workers: Arc::new(Mutex::new(Vec::new())),
        joinable_workers: Arc::new(Mutex::new(Vec::new())),
        reschedule_duration: cfg.reschedule_duration.0,
        low_priority_pool_size: cfg.low_priority_pool_size,
        pool_state_builder: Some(pool_state_builder),
    };
    (router, system)
```