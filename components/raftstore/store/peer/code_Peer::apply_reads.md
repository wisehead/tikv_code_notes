#1.Peer::apply_reads

```
Peer::apply_reads
--let states = ready.read_states().iter().map(|state| {
            let read_index_ctx = ReadIndexContext::parse(state.request_ctx.as_slice()).unwrap();
            (read_index_ctx.id, read_index_ctx.locked, state.index)
        });
--if !self.is_leader() {
----self.pending_reads.advance_replica_reads(states);
----self.post_pending_read_index_on_replica(ctx);
--} else {
----self.pending_reads.advance_leader_reads(states);        
----
```