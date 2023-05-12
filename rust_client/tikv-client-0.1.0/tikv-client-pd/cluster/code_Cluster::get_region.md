#1.Cluster::get_region

```
Cluster::get_region
--let mut req = pd_request!(self.id, pdpb::GetRegionRequest);
--req.set_region_key(key.clone());
--req.send(&self.client, timeout).await
----PdMessage::send
```