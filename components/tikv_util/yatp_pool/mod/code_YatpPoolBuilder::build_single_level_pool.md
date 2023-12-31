#1.YatpPoolBuilder::build_single_level_pool

```
YatpPoolBuilder::build_single_level_pool
--let (builder, runner) = self.create_builder();
--builder.build_with_queue_and_runner(
            yatp::queue::QueueType::SingleLevel,
            yatp::pool::CloneRunnerBuilder(runner),
        )```