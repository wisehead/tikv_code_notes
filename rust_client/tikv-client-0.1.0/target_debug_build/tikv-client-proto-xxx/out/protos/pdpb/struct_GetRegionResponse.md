#1.struct GetRegionResponse

```
#[allow(clippy::derive_partial_eq_without_eq)]
#[derive(Clone, PartialEq, ::prost::Message)]
pub struct GetRegionResponse {
    #[prost(message, optional, tag = "1")]
    pub header: ::core::option::Option<ResponseHeader>,
    #[prost(message, optional, tag = "2")]
    pub region: ::core::option::Option<super::metapb::Region>,
    #[prost(message, optional, tag = "3")]
    pub leader: ::core::option::Option<super::metapb::Peer>,
    /// Leader considers that these peers are down.
    #[prost(message, repeated, tag = "5")]
    pub down_peers: ::prost::alloc::vec::Vec<PeerStats>,
    /// Pending peers are the peers that the leader can't consider as
    /// working followers.
    #[prost(message, repeated, tag = "6")]
    pub pending_peers: ::prost::alloc::vec::Vec<super::metapb::Peer>,
}

```