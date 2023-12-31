#1.ApplyFsm::handle_apply

```
ApplyFsm::handle_apply
--if self.delegate.pending_remove || self.delegate.stopped {
            return;
        }

--if self.delegate.wait_data {
            return;
        }

```