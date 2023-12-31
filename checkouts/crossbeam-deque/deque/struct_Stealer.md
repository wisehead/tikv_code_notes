#1.struct Stealer

```rust
/// A stealer handle of a worker queue.
///
/// Stealers can be shared among threads.
///
/// Task schedulers typically have a single worker queue per worker thread.
///
/// # Examples
///
/// ```
/// use crossbeam_deque::{Steal, Worker};
///
/// let w = Worker::new_lifo();
/// w.push(1);
/// w.push(2);
///
/// let s = w.stealer();
/// assert_eq!(s.steal(), Steal::Success(1));
/// assert_eq!(s.steal(), Steal::Success(2));
/// assert_eq!(s.steal(), Steal::Empty);
/// ```
pub struct Stealer<T> {
    /// A reference to the inner representation of the queue.
    inner: Arc<CachePadded<Inner<T>>>,

    /// The flavor of the queue.
    flavor: Flavor,
}
```