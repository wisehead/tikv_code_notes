#1.struct Router

```rust
/// Router routes messages to its target FSM's mailbox.
///
/// In our abstract model, every batch system has two different kind of
/// FSMs. First is normal FSM, which does the common work like peers in a
/// raftstore model or apply delegate in apply model. Second is control FSM,
/// which does some work that requires a global view of resources or creates
/// missing FSM for specified address.
///
/// There are one control FSM and multiple normal FSMs in a system. Each FSM
/// has its own mailbox. We maintain an address book to deliver messages to the
/// specified normal FSM.
///
/// Normal FSM and control FSM can have different scheduler, but this is not
/// required.
pub struct Router<N: Fsm, C: Fsm, Ns, Cs> {
    normals: Arc<DashMap<u64, BasicMailbox<N>>>,
    pub(super) control_box: BasicMailbox<C>,
    // TODO: These two schedulers should be unified as single one. However
    // it's not possible to write FsmScheduler<Fsm=C> + FsmScheduler<Fsm=N>
    // for now.
    pub(crate) normal_scheduler: Ns,
    pub(crate) control_scheduler: Cs,

    // Number of active mailboxes.
    // Added when a mailbox is created, and subtracted it when a mailbox is
    // destroyed.
    state_cnt: Arc<AtomicUsize>,
    // Indicates the router is shutdown down or not.
    shutdown: Arc<AtomicBool>,
}
```