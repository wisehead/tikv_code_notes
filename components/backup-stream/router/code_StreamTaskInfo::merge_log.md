#1.StreamTaskInfo::merge_log

```
merge_log
-- for i in 0..files.len() {
            if batch_size >= self.merged_file_size_limit {
                Self::merge_and_flush_log_files_to(
                    storage.clone(),
                    &mut files[batch_begin_index..i],
                    metadata,
                    is_meta,
                    self.temp_file_pool.clone(),
                )
                .await?;

                batch_begin_index = i;
                batch_size = 0;
            }

            batch_size += files[i].2.length;
        }
--if batch_begin_index < files.len() {
            Self::merge_and_flush_log_files_to(
                storage.clone(),
                &mut files[batch_begin_index..],
                metadata,
                is_meta,
                self.temp_file_pool.clone(),
            )
            .await?;
        }
```