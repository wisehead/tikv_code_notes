#1.PdClient::get_region_async_opt

```
PdClient::get_region_async_opt
--self.client.unary_call_async(&METHOD_PD_GET_REGION, req, opt)
----//底层调用grpc内部库来调用METHOD_PD_GET_REGION
```

#2.METHOD_PD_GET_REGION

```rust
//由pdpb.proto生成的GetRegin()函数生成
const METHOD_PD_GET_REGION: ::grpcio::Method<GetRegionRequest, GetRegionResponse> = ::grpcio::Method {
    ty: ::grpcio::MethodType::Unary,
    name: "/pdpb.PD/GetRegion",
    req_mar: ::grpcio::Marshaller {
        ser: ::grpcio::pr_ser,
        de: ::grpcio::pr_de,
    },
    resp_mar: ::grpcio::Marshaller {
        ser: ::grpcio::pr_ser,
        de: ::grpcio::pr_de,
    },
};

```