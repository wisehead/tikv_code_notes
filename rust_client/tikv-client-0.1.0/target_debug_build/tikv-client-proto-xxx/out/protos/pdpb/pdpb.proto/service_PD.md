#1.service PD

```
service PD {
    // GetMembers get the member list of this cluster. It does not require
    // the cluster_id in request matchs the id of this cluster.
    rpc GetMembers(GetMembersRequest) returns (GetMembersResponse) {}

    rpc Tso(stream TsoRequest) returns (stream TsoResponse) {}

    rpc Bootstrap(BootstrapRequest) returns (BootstrapResponse) {}

    rpc IsBootstrapped(IsBootstrappedRequest) returns (IsBootstrappedResponse) {}

    rpc AllocID(AllocIDRequest) returns (AllocIDResponse) {}

    rpc GetStore(GetStoreRequest) returns (GetStoreResponse) {}

    rpc PutStore(PutStoreRequest) returns (PutStoreResponse) {}

    rpc GetAllStores(GetAllStoresRequest) returns (GetAllStoresResponse) {}

    rpc StoreHeartbeat(StoreHeartbeatRequest) returns (StoreHeartbeatResponse) {}

    rpc RegionHeartbeat(stream RegionHeartbeatRequest) returns (stream RegionHeartbeatResponse) {}

    rpc GetRegion(GetRegionRequest) returns (GetRegionResponse) {}

    rpc GetPrevRegion(GetRegionRequest) returns (GetRegionResponse) {}

    rpc GetRegionByID(GetRegionByIDRequest) returns (GetRegionResponse) {}

    rpc ScanRegions(ScanRegionsRequest) returns (ScanRegionsResponse) {}

    rpc AskSplit(AskSplitRequest) returns (AskSplitResponse) {
        // Use AskBatchSplit instead.
        option deprecated = true;
    }

    rpc ReportSplit(ReportSplitRequest) returns (ReportSplitResponse) {
        // Use ResportBatchSplit instead.
        option deprecated = true;
    }

    rpc AskBatchSplit(AskBatchSplitRequest) returns (AskBatchSplitResponse) {}

    rpc ReportBatchSplit(ReportBatchSplitRequest) returns (ReportBatchSplitResponse) {}

    rpc GetClusterConfig(GetClusterConfigRequest) returns (GetClusterConfigResponse) {}

    rpc PutClusterConfig(PutClusterConfigRequest) returns (PutClusterConfigResponse) {}

    rpc ScatterRegion(ScatterRegionRequest) returns (ScatterRegionResponse) {}

    rpc GetGCSafePoint(GetGCSafePointRequest) returns (GetGCSafePointResponse) {}

    rpc UpdateGCSafePoint(UpdateGCSafePointRequest) returns (UpdateGCSafePointResponse) {}

    rpc UpdateServiceGCSafePoint(UpdateServiceGCSafePointRequest) returns (UpdateServiceGCSafePointResponse) {}

    rpc SyncRegions(stream SyncRegionRequest) returns (stream SyncRegionResponse) {}

    rpc GetOperator(GetOperatorRequest) returns (GetOperatorResponse) {}

    rpc SyncMaxTS(SyncMaxTSRequest) returns (SyncMaxTSResponse) {}

    rpc SplitRegions(SplitRegionsRequest) returns (SplitRegionsResponse) {}

    rpc SplitAndScatterRegions(SplitAndScatterRegionsRequest) returns (SplitAndScatterRegionsResponse) {}

    rpc GetDCLocationInfo(GetDCLocationInfoRequest) returns (GetDCLocationInfoResponse) {}
}

```