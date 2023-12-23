#1.StoreFsmDelegate::handle_msgs

```
StoreFsmDelegate::handle_msgs
--for msg in store_msg_buf.drain(..) {
            match msg {
                StoreMsg::Start => self.on_start(),
                StoreMsg::Tick(tick) => self.on_tick(tick),
                StoreMsg::RaftMessage(msg) => self.fsm.store.on_raft_message(self.store_ctx, msg),
            }
        }
```