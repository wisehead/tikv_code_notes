#1.struct RaftRouterWrap

```rust
pub struct RaftRouterWrap<S, E> {
    router: S,
    _phantom: PhantomData<E>,
}
```