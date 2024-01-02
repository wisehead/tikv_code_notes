#1.Endpoint::prepare_min_ts

```
prepare_min_ts
--let pd_tso = pd_cli
                .get_tso()
                .await
                .map_err(|err| Error::from(err).report("failed to get tso from pd"))
                .unwrap_or_default();
--cm.update_max_ts(pd_tso);
--let min_ts = cm.global_min_lock_ts().unwrap_or(TimeStamp::max());
--Ord::min(pd_tso, min_ts)
```