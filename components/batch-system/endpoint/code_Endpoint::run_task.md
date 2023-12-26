#1.Endpoint::run_task

```
Endpoint::run_task
--match task {
            Task::WatchTask(op) => self.handle_watch_task(op),
            Task::BatchEvent(events) => self.do_backup(events),
            Task::Flush(task) => self.on_flush(task),
            Task::ModifyObserve(op) => self.on_modify_observe(op),
            Task::ForceFlush(task) => self.on_force_flush(task),
            Task::FatalError(task, err) => self.on_fatal_error(task, err),
            Task::ChangeConfig(cfg) => {
                self.on_update_change_config(cfg);
            }
            Task::Sync(cb, mut cond) => {
                if cond(self) {
                    cb()
                } else {
                    let sched = self.scheduler.clone();
                    self.pool.spawn(async move {
                        tokio::time::sleep(Duration::from_millis(500)).await;
                        sched.schedule(Task::Sync(cb, cond)).unwrap();
                    });
                }
            }
            Task::MarkFailover(t) => self.failover_time = Some(t),
            Task::FlushWithMinTs(task, min_ts) => self.on_flush_with_min_ts(task, min_ts),
            Task::RegionCheckpointsOp(s) => self.handle_region_checkpoints_op(s),
            Task::UpdateGlobalCheckpoint(task) => self.on_update_global_checkpoint(task),
        }
```