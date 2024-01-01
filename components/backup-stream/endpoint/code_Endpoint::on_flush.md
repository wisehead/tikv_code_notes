#1.Endpoint::on_flush

```
on_flush
--self.pool.block_on(async move {
            let mts = self.prepare_min_ts().await;
            info!("min_ts prepared for flushing"; "min_ts" => %mts);
            try_send!(self.scheduler, Task::FlushWithMinTs(task, mts));
        })
```