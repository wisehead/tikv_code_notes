#1.bootcluster

```
bootcluster
--let mut store = metapb::Store::default();
    store.set_id(store_id);
    if cfg.advertise_addr.is_empty() {
        store.set_address(cfg.addr.clone());
    } else {
        store.set_address(cfg.advertise_addr.clone())
    }
    if cfg.advertise_status_addr.is_empty() {
        store.set_status_address(cfg.status_addr.clone());
    } else {
        store.set_status_address(cfg.advertise_status_addr.clone())
    }
--store.set_version(env!("CARGO_PKG_VERSION").to_string());
--if let Ok(path) = std::env::current_exe() {
        if let Some(path) = path.parent() {
            store.set_deploy_path(path.to_string_lossy().to_string());
        }
    };

    store.set_start_timestamp(chrono::Local::now().timestamp());
    store.set_git_hash(
        option_env!("TIKV_BUILD_GIT_HASH")
            .unwrap_or("Unknown git hash")
            .to_string(),
    );

    let mut labels = Vec::new();
    for (k, v) in &cfg.labels {
        let mut label = metapb::StoreLabel::default();
        label.set_key(k.to_owned());
        label.set_value(v.to_owned());
        labels.push(label);
    }

--store.set_labels(labels.into());
--let region_id = pd_client
        .alloc_id()
--let peer_id = pd_client
        .alloc_id()
--let region = initial_region(store_id, region_id, peer_id);
--pd_client.bootstrap_cluster(store.clone(), region.clone()) 
```