#1.Endpoint::backup_batch

```
Endpoint::backup_batch
--let mut sw = StopWatch::by_now();
--let router = self.range_router.clone();
--let sched = self.scheduler.clone();
--let subs = self.subs.clone();
--self.pool.spawn(async move {
----let region_id = batch.region_id;
----let kvs = Self::record_batch(subs, batch);
            if kvs.as_ref().map(|x| x.is_empty()).unwrap_or(true) {
                return;
            }
----let kvs = kvs.unwrap();

            HANDLE_EVENT_DURATION_HISTOGRAM
                .with_label_values(&["to_stream_event"])
                .observe(sw.lap().as_secs_f64());
----let kv_count = kvs.len();
----let total_size = kvs.size();
            metrics::HEAP_MEMORY
                .add(total_size as _);
----utils::handle_on_event_result(&sched, router.on_events(kvs).await);
------Router::on_events
            metrics::HEAP_MEMORY
                .sub(total_size as _);
            let time_cost = sw.lap().as_secs_f64();
            if time_cost > SLOW_EVENT_THRESHOLD {
                warn!("write to temp file too slow."; "time_cost" => ?time_cost, "region_id" => %region_id, "len" => %kv_count);
            }
            HANDLE_EVENT_DURATION_HISTOGRAM
                .with_label_values(&["save_to_temp_file"])
                .observe(time_cost);
            drop(work)
        });
```