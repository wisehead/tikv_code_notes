#1.Endpoint::on_flush_with_min_ts

```
on_flush_with_min_ts
-- self.pool.spawn(self.do_flush(task, min_ts).map(|r| {
            if let Err(err) = r {
                err.report("during updating flush status")
            }
        }));
```