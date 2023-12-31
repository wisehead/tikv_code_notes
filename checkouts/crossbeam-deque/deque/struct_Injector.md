#1.struct Injector

```rust
/// An injector queue.
///
/// This is a FIFO queue that can be shared among multiple threads. Task schedulers typically have
/// a single injector queue, which is the entry point for new tasks.
///
/// # Examples
///
/// ```
/// use crossbeam_deque::{Injector, Steal};
///
/// let q = Injector::new();
/// q.push(1);
/// q.push(2);
///
/// assert_eq!(q.steal(), Steal::Success(1));
/// assert_eq!(q.steal(), Steal::Success(2));
/// assert_eq!(q.steal(), Steal::Empty);
/// ```
pub struct Injector<T> {
    /// The head of the queue.
    head: CachePadded<Position<T>>,

    /// The tail of the queue.
    tail: CachePadded<Position<T>>,

    /// Indicates that dropping a `Injector<T>` may drop values of type `T`.
    _marker: PhantomData<T>,
}
```