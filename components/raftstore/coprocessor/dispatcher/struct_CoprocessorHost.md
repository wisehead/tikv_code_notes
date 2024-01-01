#1.struct CoprocessorHost

```rust
/// Admin and invoke all coprocessors.
#[derive(Clone)]
pub struct CoprocessorHost<E>
where
    E: KvEngine + 'static,
{
    pub registry: Registry<E>,
    pub cfg: Config,
}

```