#1.struct RawTask

```rust
/// RawTask is a reference-counted `Future` task.
///
/// The reference count logic is ported from `std::sync::Arc`.
#[repr(C)]
struct RawTask<F> {
    _pin: PhantomPinned,

    ref_count: AtomicUsize,
    status: AtomicU8,
    extras: UnsafeCell<TaskExtras>,
    /// VTable of the Future.
    vtable: &'static TaskVTable,
    /// The Future itself.
    data: UnsafeCell<F>,
}
```