#1.WorkerThread::run

```
WorkerThread::run
--self.runner.start(&mut self.local);
--while !self.local.core().is_shutdown() {
----let task = match self.pop() {
                Some(t) => t,
                None => continue,
            };
----self.runner.handle(&mut self.local, task.task_cell);
        }
--self.runner.end(&mut self.local);

        // Drain all futures in the queue
--while self.local.pop().is_some() {}
```