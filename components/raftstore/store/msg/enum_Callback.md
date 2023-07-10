#1.enum Callback

```rust
/// Variants of callbacks for `Msg`.
///  - `Read`: a callback for read only requests including `StatusRequest`,
///    `GetRequest` and `SnapRequest`
///  - `Write`: a callback for write only requests including `AdminRequest`
///    `PutRequest`, `DeleteRequest` and `DeleteRangeRequest`.
pub enum Callback<S: Snapshot> {
    /// No callback.
    None,
    /// Read callback.
    Read {
        cb: BoxReadCallback<S>,

        tracker: TrackerToken,
    },
    /// Write callback.
    Write {
        cb: BoxWriteCallback,
        /// `proposed_cb` is called after a request is proposed to the raft
        /// group successfully. It's used to notify the caller to move on early
        /// because it's very likely the request will be applied to the
        /// raftstore.
        proposed_cb: Option<ExtCallback>,
        /// `committed_cb` is called after a request is committed and before
        /// it's being applied, and it's guaranteed that the request will be
        /// successfully applied soon.
        committed_cb: Option<ExtCallback>,

        trackers: SmallVec<[TimeTracker; 4]>,
    },
    #[cfg(any(test, feature = "testexport"))]
    /// Test purpose callback
    Test { cb: TestCallback },
}

```