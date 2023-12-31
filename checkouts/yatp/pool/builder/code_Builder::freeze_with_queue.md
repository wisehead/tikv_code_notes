#1.Builder::freeze_with_queue

```
Builder::freeze_with_queue
--let (injector, local_queues) = queue::build(queue_type, self.sched_config.max_thread_count);
--let core = Arc::new(QueueCore::new(injector, self.sched_config.clone()));

--(
            Remote::new(core.clone()),
            LazyBuilder {
                builder: self.clone(),
                core,
                local_queues,
            },
        )
```