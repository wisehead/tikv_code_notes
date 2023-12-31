#1.enum LocalQueueInner

```rust
enum LocalQueueInner<T> {
    SingleLevel(single_level::LocalQueue<T>),
    Multilevel(multilevel::LocalQueue<T>),
    Priority(priority::LocalQueue<T>),
}
```