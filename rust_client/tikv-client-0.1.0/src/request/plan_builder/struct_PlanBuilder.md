#1.struct PlanBuilder

```
/// Builder type for plans (see that module for more).
pub struct PlanBuilder<PdC: PdClient, P: Plan, Ph: PlanBuilderPhase> {
    pd_client: Arc<PdC>,
    plan: P,
    phantom: PhantomData<Ph>,
}
```