#1.BackupStreamObserver::new

```
    /// Create a new `BackupStreamObserver`.
    ///
    /// Events are strong ordered, so `scheduler` must be implemented as
    /// a FIFO queue.
BackupStreamObserver::new
--BackupStreamObserver {
            scheduler,
            ranges: Default::default(),
        }
```