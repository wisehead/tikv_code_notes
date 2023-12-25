#1.new_engine_opt

```
new_engine_opt
--let mut cf_opts: Vec<_> = cf_opts
        .into_iter()
        .map(|(name, opt)| (name, opt.into_raw()))
        .collect();
--if !db_exist(path) {
----db_opt.create_if_missing(true);
----db_opt.create_missing_column_families(true);
----let db = DB::open_cf(db_opt, path, cf_opts.into_iter().collect()).map_err(r2e)?;
----return Ok(RocksEngine::new(db));  
--db_opt.create_if_missing(false);
--let cfs_list = DB::list_column_families(&db_opt, path).map_err(r2e)?;
--let existed: Vec<&str> = cfs_list.iter().map(|v| v.as_str()).collect();
--let needed: Vec<&str> = cf_opts.iter().map(|(name, _)| *name).collect();      
--let cf_descs = if !existed.is_empty() {
        let env = match db_opt.env() {
            Some(env) => env,
            None => Arc::new(Env::default()),
        };
        // panic if OPTIONS not found for existing instance?
        let (_, tmp) = load_latest_options(path, &env, true)
            .unwrap_or_else(|e| panic!("failed to load_latest_options {:?}", e))
            .unwrap_or_else(|| panic!("couldn't find the OPTIONS file"));
        tmp
    } else {
        vec![]
    };

--for cf in &existed {
        if cf_opts.iter().all(|(name, _)| name != cf) {
            cf_opts.push((cf, ColumnFamilyOptions::default()));
        }
    }
--for (name, opt) in &mut cf_opts {
        adjust_dynamic_level_bytes(&cf_descs, name, opt);
    }
--let cfds: Vec<_> = cf_opts.into_iter().collect();
--if needed.len() == existed.len() && needed.len() == cfds.len() {
----let db = DB::open_cf(db_opt, path, cfds).map_err(r2e)?;
----return Ok(RocksEngine::new(db));
--db_opt.create_missing_column_families(true);
--let mut db = DB::open_cf(db_opt, path, cfds).map_err(r2e)?;
--for cf in cfs_diff(&existed, &needed) {
        // We have checked it at the very beginning, so it must be needed.
        assert_ne!(cf, CF_DEFAULT);
        db.drop_cf(cf).map_err(r2e)?;
    }

--Ok(RocksEngine::new(db))
```