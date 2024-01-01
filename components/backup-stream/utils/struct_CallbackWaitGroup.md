#1.struct CallbackWaitGroup

```rust
pub struct CallbackWaitGroup {
    running: AtomicUsize,
    on_finish_all: std::sync::Mutex<Vec<Box<dyn FnOnce() + Send + 'static>>>,
}
```