#1.struct Store

```rust
pub struct Store {
    id: u64,
    // Unix time when it's started.
    start_time: Option<u64>,
    logger: Logger,
}

impl Store {
    pub fn new(id: u64, logger: Logger) -> Store {
        Store {
            id,
            start_time: None,
            logger: logger.new(o!("store_id" => id)),
        }
    }

    pub fn store_id(&self) -> u64 {
        self.id
    }

    pub fn start_time(&self) -> Option<u64> {
        self.start_time
    }

    pub fn logger(&self) -> &Logger {
        &self.logger
    }
}
```