#1.Peer::read_index_follower

```
Peer::read_index_follower
--let request = req
            .mut_requests()
            .get_mut(0)
            .filter(|req| req.has_read_index())
            .map(|req| req.take_read_index());
--let (id, _dropped) = propose_read_index(self.raft_group_mut(), request.as_ref());
--let mut read = ReadIndexRequest::with_command(id, req, ch, now);
--read.addition_request = request.map(Box::new);
--self.pending_reads_mut().push_back(read, false);            
--self.set_has_ready();
```