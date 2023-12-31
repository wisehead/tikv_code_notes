#1.single_level

```
single_level
--let (injector, locals) = single_level::create(local_num);
--(
        TaskInjector(InjectorInner::SingleLevel(injector)),
        locals
            .into_iter()
            .map(|i| LocalQueue(LocalQueueInner::SingleLevel(i)))
            .collect(),
    )
```