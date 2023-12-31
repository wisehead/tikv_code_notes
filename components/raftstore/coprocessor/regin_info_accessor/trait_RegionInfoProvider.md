#1.trait RegionInfoProvider

```rust
pub trait RegionInfoProvider: Send + Sync {
    /// Get a iterator of regions that contains `from` or have keys larger than
    /// `from`, and invoke the callback to process the result.
    fn seek_region(&self, _from: &[u8], _callback: SeekRegionCallback) -> Result<()> {
        unimplemented!()
    }

    fn find_region_by_id(
        &self,
        _reigon_id: u64,
        _callback: Callback<Option<RegionInfo>>,
    ) -> Result<()> {
        unimplemented!()
    }

    fn find_region_by_key(&self, _key: &[u8]) -> Result<Region> {
        unimplemented!()
    }

    fn get_regions_in_range(&self, _start_key: &[u8], _end_key: &[u8]) -> Result<Vec<Region>> {
        unimplemented!()
    }
}
```