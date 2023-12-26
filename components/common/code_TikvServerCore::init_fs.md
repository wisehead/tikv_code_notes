#1.TikvServerCore::init_fs

```
TikvServerCore::init_fs
--let lock_path = self.store_path.join(Path::new("LOCK"));
--let f = File::create(lock_path.as_path())
            .unwrap_or_else(|e| fatal!("failed to create lock at {}: {}", lock_path.display(), e));
--f.try_lock_exclusive()
-- self.lock_files.push(f);
--let disk_stats = fs2::statvfs(&self.config.storage.data_dir).unwrap();
--let mut capacity = disk_stats.total_space();
--if self.config.raft_store.capacity.0 > 0 {
            capacity = cmp::min(capacity, self.config.raft_store.capacity.0);
        }
        // reserve space for kv engine
--let kv_reserved_size =
            calculate_reserved_space(capacity, self.config.storage.reserve_space.0);
--disk::set_disk_reserved_space(kv_reserved_size);
--reserve_physical_space(
            &self.config.storage.data_dir,
            disk_stats.available_space(),
            kv_reserved_size,
        );
--let raft_data_dir = if self.config.raft_engine.enable {
            self.config.raft_engine.config().dir
        } else {
            self.config.raft_store.raftdb_path.clone()
        };        
--let separated_raft_mount_path =
            path_in_diff_mount_point(&self.config.storage.data_dir, &raft_data_dir);
--if separated_raft_mount_path {
            let raft_disk_stats = fs2::statvfs(&raft_data_dir).unwrap();
            // reserve space for raft engine if raft engine is deployed separately
            let raft_reserved_size = calculate_reserved_space(
                raft_disk_stats.total_space(),
                self.config.storage.reserve_raft_space.0,
            );
            disk::set_raft_disk_reserved_space(raft_reserved_size);
            reserve_physical_space(
                &raft_data_dir,
                raft_disk_stats.available_space(),
                raft_reserved_size,
            );
        }
```

#2.calculate_reserved_space

```
calculate_reserved_space
--let mut reserved_size = reserved_size_from_config;
--if reserved_size_from_config != 0 {
----reserved_size =cmp::max((capacity as f64 * 0.05) as u64, reserved_size_from_config);
```

#3.reserve_physical_space

```
reserve_physical_space
--file_system::remove_file(path)
--if available > reserved_size {
----file_system::reserve_space_for_recover(data_dir, reserved_size / 5)
                    .map_err(|e| panic!("Failed to reserve space for recovery: {}.", e))
                    .unwrap();
--} else {
----warn!("no enough disk space left to create the place holder file");
```