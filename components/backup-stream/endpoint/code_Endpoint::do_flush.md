#1.Endpoint::do_flush

```
do_flush
--let get_rts = self.get_resolved_regions(min_ts);
--let router = self.range_router.clone();
--let store_id = self.store_id;
--let mut flush_ob = self.flush_observer();
--async move {
            let mut resolved = get_rts.await?;
            let mut new_rts = resolved.global_checkpoint();
            fail::fail_point!("delay_on_flush");
            flush_ob.before(resolved.take_resolve_result()).await;
            if let Some(rewritten_rts) = flush_ob.rewrite_resolved_ts(&task).await {
                info!("rewriting resolved ts"; "old" => %new_rts, "new" => %rewritten_rts);
                new_rts = rewritten_rts.min(new_rts);
            }
            if let Some(rts) = router.do_flush(&task, store_id, new_rts).await {
                info!("flushing and refreshing checkpoint ts.";
                    "checkpoint_ts" => %rts,
                    "task" => %task,
                );
                if rts == 0 {
                    // We cannot advance the resolved ts for now.
                    return Ok(());
                }
                flush_ob.after(&task, rts).await?
            }
            Ok(())
--}
```