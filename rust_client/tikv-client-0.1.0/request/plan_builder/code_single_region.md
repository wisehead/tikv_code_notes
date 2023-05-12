#1.single_region

```
single_region
--let key = self.plan.request.key();
--let store = self.pd_client.clone().store_for_key(key.into()).await?;
--set_single_region_store(self.plan, store, self.pd_client)
```