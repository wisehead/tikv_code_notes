#1.struct Client

```
/// The TiKV raw `Client` is used to interact with TiKV using raw requests.
///
/// Raw requests don't need a wrapping transaction.
/// Each request is immediately processed once executed.
///
/// The returned results of raw request methods are [`Future`](std::future::Future)s that must be
/// awaited to execute.
#[derive(Clone)]
pub struct Client {
    rpc: Arc<PdRpcClient>,
    cf: Option<ColumnFamily>,
    /// Whether to use the [`atomic mode`](Client::with_atomic_for_cas).
    atomic: bool,
}

```