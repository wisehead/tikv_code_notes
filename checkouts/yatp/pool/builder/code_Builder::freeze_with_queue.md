#1.Builder::freeze_with_queue

```

    /// Freezes the configurations and returns the task scheduler and
    /// a builder to for lazy spawning threads.
    ///
    /// `queue_builder` is a closure that creates a task queue. It accepts the
    /// number of local queues and returns the task injector and local queues.
    ///
    /// In some cases, especially building up a large application, a task
    /// scheduler is required before spawning new threads. You can use this
    /// to separate the construction and starting.
    
Builder::freeze_with_queue
--let (injector, local_queues) = queue::build(queue_type, self.sched_config.max_thread_count);
----single_level
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