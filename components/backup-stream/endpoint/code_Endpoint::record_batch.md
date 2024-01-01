#1.Endpoint::record_batch
```
Endpoint::record_batch
--let region_id = batch.region_id;
--let mut resolver = match subs.get_subscription_of(region_id) {
            Some(rts) => rts,
            None => {
                debug!("the region isn't registered (no resolver found) but sent to backup_batch, maybe stale."; "region_id" => %region_id);
                return None;
            }
        };
        // Stale data is acceptable, while stale locks may block the checkpoint
        // advancing.
        // 
        // Let L be the instant some key locked, U be the instant it unlocked,
        // +---------*-------L-----------U--*-------------+
        //           ^   ^----(1)----^      ^ We get the snapshot for initial scanning at here.
        //           +- If we issue refresh resolver at here, and the cmd batch (1) is the last cmd batch of the first observing.
        //              ...the background initial scanning may keep running, and the lock would be sent to the scanning.
        //              ...note that (1) is the last cmd batch of first observing, so the unlock event would never be sent to us.
        //              ...then the lock would get an eternal life in the resolver :|
        //                 (Before we refreshing the resolver for this region again)
        // 
        
--if batch.pitr_id != resolver.value().handle.id {
            debug!("stale command"; "region_id" => %region_id, "now" => ?resolver.value().handle.id, "remote" => ?batch.pitr_id);
            return None;
        }

--let kvs = ApplyEvents::from_cmd_batch(batch, resolver.value_mut().resolver());
--Some(kvs)
```