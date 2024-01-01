#1.Endpoint::do_backup

```
Endpoint::do_backup
--let wg = CallbackWaitGroup::new();
--for batch in events {
----self.backup_batch(batch, wg.clone().work());
        }
--self.pool.block_on(wg.wait())
```