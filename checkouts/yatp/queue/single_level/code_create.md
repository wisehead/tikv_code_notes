#1.create

```
create
--let injector = Arc::new(Injector::new());
--let workers: Vec<_> = iter::repeat_with(Worker::new_lifo)
        .take(local_num)
        .collect();
--let stealers: Vec<_> = workers.iter().map(Worker::stealer).collect();
--let local_queues = workers
        .into_iter()
        .enumerate()
        .map(|(self_index, local_queue)| {
            let mut stealers: Vec<_> = stealers
                .iter()
                .enumerate()
                .filter(|(index, _)| *index != self_index)
                .map(|(_, stealer)| stealer.clone())
                .collect();
            // Steal with a random start to avoid imbalance.
            stealers.shuffle(&mut thread_rng());
            LocalQueue {
                local_queue,
                injector: injector.clone(),
                stealers,
            }
        })
        .collect();

    (TaskInjector(injector), local_queues)
```