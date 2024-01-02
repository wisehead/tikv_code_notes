#1.StreamTaskInfo::merge_and_flush_log_files_to

```
merge_and_flush_log_files_to
--for (_, data_file, file_info) in files {
            let mut file_info_clone = file_info.to_owned();
            // Update offset of file_info(DataFileInfo)
            //  and push it into merged_file_info(DataFileGroup).
            file_info_clone.set_range_offset(stat_length);
            data_files_open.push({
                let file = shared_pool
                    .open_raw_for_read(data_file.inner.path())
                    .context(format_args!(
                        "failed to open read file {:?}",
                        data_file.inner.path()
                    ))?;
                let compress_length = file.len().await?;
                stat_length += compress_length;
                file_info_clone.set_range_length(compress_length);
                file
            });
            data_file_infos.push(file_info_clone);

            let rts = file_info.resolved_ts;
            min_resolved_ts = min_resolved_ts.map_or(Some(rts), |r| Some(r.min(rts)));
            min_ts = min_ts.map_or(Some(file_info.min_ts), |ts| Some(ts.min(file_info.min_ts)));
            max_ts = max_ts.map_or(Some(file_info.max_ts), |ts| Some(ts.max(file_info.max_ts)));
        }
--let min_ts = min_ts.unwrap_or_default();
--let max_ts = max_ts.unwrap_or_default();
--merged_file_info.set_path(TempFileKey::file_name(
            metadata.store_id,
            min_ts,
            max_ts,
            is_meta,
        ));
--merged_file_info.set_data_files_info(data_file_infos.into());
--merged_file_info.set_length(stat_length);
--merged_file_info.set_max_ts(max_ts);
--merged_file_info.set_min_ts(min_ts);
--merged_file_info.set_min_resolved_ts(min_resolved_ts.unwrap_or_default());
```