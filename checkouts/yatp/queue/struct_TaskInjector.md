#1.struct TaskInjector

```rust
/// The injector of a task queue.
pub(crate) struct TaskInjector<T>(InjectorInner<T>);
```