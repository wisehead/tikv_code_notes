#1.struct InjectorInner

```rust
enum InjectorInner<T> {
    SingleLevel(single_level::TaskInjector<T>),
    Multilevel(multilevel::TaskInjector<T>),
    Priority(priority::TaskInjector<T>),
}
```