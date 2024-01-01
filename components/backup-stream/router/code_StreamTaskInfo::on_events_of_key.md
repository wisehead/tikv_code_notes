#1.StreamTaskInfo::on_events_of_key

```
StreamTaskInfo::on_events_of_key
--if let Some(f) = self.files.read().await.get(&key) {
            self.total_size
                .fetch_add(f.lock().await.on_events(events).await?, Ordering::SeqCst);
            return Ok(());
        }
--// slow path: try to insert the element.
--let mut w = self.files.write().await;
        // double check before insert. there may be someone already insert that
        // when we are waiting for the write lock.
        // silence the lint advising us to use the `Entry` API which may introduce
        // copying.
        #[allow(clippy::map_entry)]
--if !w.contains_key(&key) {
            let path = key.temp_file_name();
            let val = Mutex::new(DataFile::new(path, &self.temp_file_pool).await?);
            w.insert(key, val);
        }

--let f = w.get(&key).unwrap();
--self.total_size
            .fetch_add(f.lock().await.on_events(events).await?, Ordering::SeqCst);        
```