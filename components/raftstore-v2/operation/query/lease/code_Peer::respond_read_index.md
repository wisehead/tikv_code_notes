#1.Peer::respond_read_index

```
Peer::respond_read_index
--for (_, ch, mut read_index) in read_index_req.take_cmds().drain(..) {
----match (read_index, read_index_req.read_index) {
------(Some(local_responsed_index), Some(batch_index)) => {
                    // `read_index` could be less than `read_index_req.read_index` because the
                    // former is filled with `committed index` when
                    // proposed, and the latter is filled
                    // after a read-index procedure finished.
                    read_index = Some(std::cmp::max(local_responsed_index, batch_index));
                }
------(None, _) => {
                    // Actually, the read_index is none if and only if it's the first one in
                    // read_index_req.cmds. Starting from the second, all the following ones'
                    // read_index is not none.
                    read_index = read_index_req.read_index;
                }
                _ => {}
            }
----let read_resp = ReadResponse::new(read_index.unwrap_or(0));
----ch.set_result(QueryResult::Read(read_resp));
```