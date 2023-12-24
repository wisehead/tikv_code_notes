#1.struct BasicMailbox

```rust
/// A basic mailbox.
///
/// A mailbox holds an FSM owner, and the sending end of a channel to send
/// messages to that owner. Multiple producers share the same mailbox to
/// communicate with a FSM.
///
/// The mailbox's FSM owner needs to be scheduled to a [`Poller`] to handle its
/// pending messages. Therefore, the producer of messages also needs to provide
/// a channel to a poller ([`FsmScheduler`]), so that the mailbox can schedule
/// its FSM owner. When a message is sent to a mailbox, the mailbox will check
/// whether its FSM owner is idle, i.e. not already taken and scheduled. If the
/// FSM is idle, it will be scheduled immediately. By doing so, the mailbox
/// temporarily transfers its ownership of the FSM to the poller. The
/// implementation must make sure the same FSM is returned afterwards via the
/// [`release`] method.
///
/// [`Poller`]: crate::batch::Poller
pub struct BasicMailbox<Owner: Fsm> {
    sender: mpsc::LooseBoundedSender<Owner::Message>,
    state: Arc<FsmState<Owner>>,
}
```