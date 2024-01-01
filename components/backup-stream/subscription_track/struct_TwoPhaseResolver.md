#1.struct TwoPhaseResolver

```rust
/// This enhanced version of `Resolver` allow some unordered lock events.
/// The name "2-phase" means this is used for 2 *concurrency* phases of
/// observing a region:
/// 1. Doing the initial scanning.
/// 2. Listening at the incremental data.
///
/// ```text
/// +->(Start TS Of Task)            +->(Task registered to KV)
/// +--------------------------------+------------------------>
/// ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^ ^~~~~~~~~~~~~~~~~~~~~~~~~
/// |                                 +-> Phase 2: Listening incremental data.
/// +-> Phase 1: Initial scanning scans writes between start ts and now.
/// ```
///
/// In backup-stream, we execute these two tasks parallel. Which may make some
/// race conditions:
/// - When doing initial scanning, there may be a flush triggered, but the
///   default resolver would probably resolved to the tip of incremental events.
/// - When doing initial scanning, we meet and track a lock already meet by the
///   incremental events, then the default resolver cannot untrack this lock any
///   more.
///
/// This version of resolver did some change for solve these problems:
/// - The resolver won't advance the resolved ts to greater than `stable_ts` if
///   there is some. This can help us prevent resolved ts from advancing when
///   initial scanning hasn't finished yet.
/// - When we `untrack` a lock haven't been tracked, this would record it, and
///   skip this lock if we want to track it then. This would be safe because:
///   - untracking a lock not be tracked is no-op for now.
///   - tracking a lock have already being untracked (unordered call of `track`
///     and `untrack`) wouldn't happen at phase 2 for same region. but only when
///     phase 1 and phase 2 happened concurrently, at that time, we wouldn't and
///     cannot advance the resolved ts.
pub struct TwoPhaseResolver {
    resolver: Resolver,
    future_locks: Vec<FutureLock>,
    /// When `Some`, is the start ts of the initial scanning.
    /// And implies the phase 1 (initial scanning) is keep running
    /// asynchronously.
    stable_ts: Option<TimeStamp>,
}

```