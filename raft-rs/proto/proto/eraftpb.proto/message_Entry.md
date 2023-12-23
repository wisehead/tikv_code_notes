#1.message Entry

```rust
// The entry is a type of change that needs to be applied. It contains two data fields.
// While the fields are built into the model; their usage is determined by the entry_type.
//
// For normal entries, the data field should contain the data change that should be applied.
// The context field can be used for any contextual data that might be relevant to the
// application of the data.
//
// For configuration changes, the data will contain the ConfChange message and the
// context will provide anything needed to assist the configuration change. The context
// if for the user to set and use in this case.
message Entry {
    EntryType entry_type = 1;
    uint64 term = 2;
    uint64 index = 3;
    bytes data = 4;
    bytes context = 6;

    // Deprecated! It is kept for backward compatibility.
    // TODO: remove it in the next major release.
    bool sync_log = 5;
}
```