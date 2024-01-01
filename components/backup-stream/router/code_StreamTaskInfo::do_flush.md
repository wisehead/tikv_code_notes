#1.StreamTaskInfo::do_flush

```
do_flush
--// generate meta data and prepare to flush to storage
--let mut metadata_info = self
                .move_to_flushing_files()
                .await?
                .generate_metadata(store_id)
                .await?;
```