#1.struct TaskExtras

```rust
struct TaskExtras {
    extras: Extras,
    remote: Option<WeakRemote<TaskCell>>,
}
```