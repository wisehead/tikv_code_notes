#1.StreamTaskInfo::on_events

```
StreamTaskInfo::on_events
--futures::future::try_join_all(kv.partition_by_table_key().into_iter().map(
            |(key, events)| {
                self.on_events_of_key(key, events)
                    .map(move |r| r.context(format_args!("when handling the file key {:?}", key)))
            },
        ))
        .await?;
```

#2.ApplyEvents::partition_by_table_key

```
partition_by_table_key
--self.group_by(move |event| Some(TempFileKey::of(event, region_id)))
```