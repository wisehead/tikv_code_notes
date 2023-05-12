#1.RetryClient::get_region

```
get_region
--retry!(self, "get_region", |cluster| {
            let key = key.clone();
            async {
                cluster
                    .get_region(key.clone(), self.timeout)
                    .await
                    .and_then(|resp| {
                        region_from_response(resp, || Error::RegionForKeyNotFound { key })
                    })
            }
        })
```