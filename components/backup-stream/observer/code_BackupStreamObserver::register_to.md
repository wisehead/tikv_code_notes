#1.BackupStreamObserver::register_to

```
BackupStreamObserver::register_to
--let registry = &mut coprocessor_host.registry;
--registry.register_cmd_observer(0, BoxCmdObserver::new(self.clone()));
--registry.register_role_observer(100, BoxRoleObserver::new(self.clone()));
--registry.register_region_change_observer(100, BoxRegionChangeObserver::new(self.clone()));
```