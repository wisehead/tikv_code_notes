#1.request_to_triple

```
request_to_triple
--let (key, value, cf) = match req.get_cmd_type() {
        CmdType::Put => {
            let mut put = req.take_put();
            (put.take_key(), put.take_value(), put.cf)
        }
        CmdType::Delete => {
            let mut del = req.take_delete();
            (del.take_key(), Vec::new(), del.cf)
        }
        _ => return Either::Right(req),
    };
--Either::Left((key, value, cf_name(cf.as_str())))
```