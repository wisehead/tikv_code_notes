#1.LazyWorker::new

```
LazyWorker::new
--let name = name.into();
--let worker = Worker::new(name.clone());
----Builder::new(name).create()
--worker.lazy_build(name)
```