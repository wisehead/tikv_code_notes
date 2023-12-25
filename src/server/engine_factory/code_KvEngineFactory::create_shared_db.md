#1.KvEngineFactory::create_shared_db

```
KvEngineFactory::create_shared_db
--let path = path.as_ref();
--let mut db_opts = self.db_opts(EngineType::RaftKv);
--let cf_opts = self.cf_opts(None, EngineType::RaftKv);
--if let Some(listener) = &self.inner.flow_listener {
----db_opts.add_event_listener(listener.clone());
        }
--let target_path = path.join(DEFAULT_ROCKSDB_SUB_DIR);
--let kv_engine =
            engine_rocks::util::new_engine_opt(target_path.to_str().unwrap(), db_opts, cf_opts);
```