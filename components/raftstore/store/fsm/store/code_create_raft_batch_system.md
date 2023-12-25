#1.create_raft_batch_system

```
create_raft_batch_system
--let (store_tx, store_fsm) = StoreFsm::new(cfg);
--let (apply_router, apply_system) = create_apply_batch_system(
        cfg,
        resource_manager
            .as_ref()
            .map(|m| m.derive_controller("apply".to_owned(), false)),
    );
--let (router, system) = batch_system::create_system(
        &cfg.store_batch_system,
        store_tx,
        store_fsm,
        None, // Do not do priority scheduling for store batch system
    );
```