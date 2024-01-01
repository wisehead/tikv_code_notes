#1.YatpPoolRunner::handle

```
YatpPoolRunner::handle
--let extras = task_cell.mut_extras();
--if let Some(schedule_time) = extras.schedule_time() {
            let idx = (*self.metric_idx_from_task_meta)(extras.metadata());
            self.schedule_wait_durations[idx].observe(schedule_time.elapsed().as_secs_f64());
        }
--let finished = self.inner.handle(local, task_cell);
--if self.ticker.try_tick() {
            self.schedule_wait_durations.iter().for_each(|m| m.flush());
        }
```