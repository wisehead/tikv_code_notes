#1.StreamTaskInfo::do_flush

```
do_flush
--// generate meta data and prepare to flush to storage
--let mut metadata_info = self
                .move_to_flushing_files()
                .await?
                .generate_metadata(store_id)
                .await?;
--// flush log file to storage.
--self.flush_log(&mut metadata_info).await?;
--
```


#2.StreamTaskInfo::flush_log

```
flush_log
--let storage = self.storage.clone();
--self.merge_log(metadata, storage.clone(), &self.flushing_files, false)
            .await?;
--self.merge_log(metadata, storage.clone(), &self.flushing_meta_files, true)
            .await?;
```