#1.enum FlowController

```rust
pub enum FlowController {
    Singleton(EngineFlowController),
    Tablet(TabletFlowController),
}
```