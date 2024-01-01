#1.LazyWorker::start

```
LazyWorker::start
--if let Some(receiver) = self.receiver.take() {
----self.worker
                .start_impl(runner, receiver, self.metrics_pending_task_count.clone());
            return true;
        }
```