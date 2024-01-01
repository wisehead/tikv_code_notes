#1.Scheduler::schedule

```
Scheduler::schedule
--self.schedule_force(task)
```

#2.Scheduler::schedule_force

```
Scheduler::schedule_force
--if let Err(e) = self.sender.unbounded_send(Msg::Task(task)) {
            if let Msg::Task(t) = e.into_inner() {
                self.counter.fetch_sub(1, Ordering::SeqCst);
                self.metrics_pending_task_count.dec();
                return Err(ScheduleError::Stopped(t));
            }
        }
```