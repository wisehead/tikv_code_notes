#1.Peer::response_read

```
Peer::response_read
--for (req, cb, mut read_index) in read.take_cmds().drain(..) {
----if let Some(locked) = read.locked.take() {
                let mut response = raft_cmdpb::Response::default();
                response.mut_read_index().set_locked(*locked);
                let mut cmd_resp = RaftCmdResponse::default();
                cmd_resp.mut_responses().push(response);
                cb.invoke_read(ReadResponse {
                    response: cmd_resp,
                    snapshot: None,
                    txn_extra_op: TxnExtraOp::Noop,
                });
                continue;
--if req.get_header().get_replica_read() {
                // We should check epoch since the range could be changed.
----cb.invoke_read(self.handle_read(ctx, req, true, read.read_index));
------Peer::handle_read
--} else {
                // The request could be proposed when the peer was leader.
                // TODO: figure out that it's necessary to notify stale or not.
                let term = self.term();
                apply::notify_stale_req(term, cb);
            }
```