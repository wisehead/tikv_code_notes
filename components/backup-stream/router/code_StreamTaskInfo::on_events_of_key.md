#1.StreamTaskInfo::on_events_of_key

```
StreamTaskInfo::on_events_of_key
--if let Some(f) = self.files.read().await.get(&key) {
            self.total_size
                .fetch_add(f.lock().await.on_events(events).await?, Ordering::SeqCst);
            return Ok(());
        }
```