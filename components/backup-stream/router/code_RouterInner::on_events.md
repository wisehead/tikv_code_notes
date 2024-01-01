#1.RouterInner::on_events

```
RouterInner::on_events
--let partitioned_events = kv.partition_by_range(&self.ranges.rl());
--let partitioned_events = kv.partition_by_range(&self.ranges.rl());
--let tasks = partitioned_events
            .into_iter()
            .map(|(task, events)| self.on_event(task.clone(), events).map(move |r| (task, r)));
--futures::future::join_all(tasks).await
```