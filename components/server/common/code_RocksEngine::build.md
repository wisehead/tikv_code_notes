#1.RocksEngine::build

```
RocksEngine::build
--let mut raft_data_state_machine = RaftDataStateMachine::new(
            &config.storage.data_dir,
            &config.raft_engine.config().dir,
            &config.raft_store.raftdb_path,
        );
        let should_dump = raft_data_state_machine.before_open_target();

        let raft_db_path = &config.raft_store.raftdb_path;
        let config_raftdb = &config.raftdb;
        let statistics = Arc::new(RocksStatistics::new_titan());
        let raft_db_opts = config_raftdb.build_opt(env.clone(), Some(&statistics));
        let raft_cf_opts = config_raftdb.build_cf_opts(block_cache);
        let raftdb = engine_rocks::util::new_engine_opt(raft_db_path, raft_db_opts, raft_cf_opts)
            .expect("failed to open raftdb");
--if should_dump {
            let raft_engine =
                RaftLogEngine::new(config.raft_engine.config(), key_manager.clone(), None)
                    .expect("failed to open raft engine for migration");
            dump_raft_engine_to_raftdb(&raft_engine, &raftdb, 8 /* threads */);
            raft_engine.stop();
            drop(raft_engine);
            raft_data_state_machine.after_dump_data();
        }
--(raftdb, Some(statistics))            
```