#1.ConfigController::register

```
ConfigController::register
--let mut inner = self.inner.write().unwrap();
--if inner.config_mgrs.insert(module.clone(), cfg_mgr).is_some() {
            warn!("config manager for module {:?} already registered", module)
        }
```