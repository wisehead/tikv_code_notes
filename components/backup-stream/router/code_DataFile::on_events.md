#1.DataFile::on_events

```
DataFile::on_events
-- for mut event in events.events {
            let encoded = EventEncoder::encode_event(&event.key, &event.value);
            let mut size = 0;
            for slice in encoded {
                let slice = slice.as_ref();
                self.inner.write_all(slice).await?;
                self.sha256.update(slice).map_err(|err| {
                    Error::Other(box_err!("openssl hasher failed to update: {}", err))
                })?;
                size += slice.len();
            }
            let key = Key::from_encoded(std::mem::take(&mut event.key));
            let ts = key.decode_ts().expect("key without ts");
            total_size += size;
            self.min_ts = self.min_ts.min(ts);
            self.max_ts = self.max_ts.max(ts);
            self.resolved_ts = self.resolved_ts.max(events.region_resolved_ts.into());

            // decode_begin_ts is used to maintain the txn when restore log.
            // if value is empty, no need to decode begin_ts.
            if event.cf == CF_WRITE && !event.value.is_empty() {
                let begin_ts = Self::decode_begin_ts(event.value)?;
                self.min_begin_ts = Some(self.min_begin_ts.map_or(begin_ts, |ts| ts.min(begin_ts)));
            }
            self.number_of_entries += 1;
            self.file_size += size;
            self.update_key_bound(key.into_encoded());
        }
```