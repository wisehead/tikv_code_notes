#1.store_for_key

```
store_for_key
--let region = self.region_for_key(key).await?;
--self.map_region_to_store(region).await

```