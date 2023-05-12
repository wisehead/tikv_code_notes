#1.struct RawPutRequest

```
#[allow(clippy::derive_partial_eq_without_eq)]
#[derive(Clone, PartialEq, ::prost::Message)]
pub struct RawPutRequest {
    #[prost(message, optional, tag = "1")]
    pub context: ::core::option::Option<Context>,
    #[prost(bytes = "vec", tag = "2")]
    pub key: ::prost::alloc::vec::Vec<u8>,
    #[prost(bytes = "vec", tag = "3")]
    pub value: ::prost::alloc::vec::Vec<u8>,
    #[prost(string, tag = "4")]
    pub cf: ::prost::alloc::string::String,
    #[prost(uint64, tag = "5")]
    pub ttl: u64,
    #[prost(bool, tag = "6")]
    pub for_cas: bool,
}
```