#1.create_apply_batch_system

```
create_apply_batch_system
--let (control_tx, control_fsm) = ControlFsm::new();
--let (router, system) = batch_system::create_system(
        &cfg.apply_batch_system,
        control_tx,
        control_fsm,
        resource_ctl,
    );
--(ApplyRouter { router }, ApplyBatchSystem { system })
```