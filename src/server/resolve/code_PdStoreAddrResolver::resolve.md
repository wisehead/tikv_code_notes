#1.PdStoreAddrResolver::resolve

```
PdStoreAddrResolver::resolve
--let task = Task { store_id, cb };
--box_try!(self.sched.schedule(task));
----Scheduler::schedule
```