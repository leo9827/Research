# Beam model
Apache [[Beam]] is a unified model for defining both batch and streaming data-parallel processing pipelines. To get started with Beam, you’ll need to understand an important set of core concepts:  
Apache Beam 是一个用于定义批处理和流数据并行处理管道的统一模型。要开始使用 Beam，需要了解一组重要的核心概念：

- [_Pipeline_](https://beam.apache.org/documentation/basics/#pipeline) - A pipeline is a user-constructed graph of transformations that defines the desired data processing operations.  
    管道 - 管道是用户构建的转换图，定义所需的数据处理操作。
- [_PCollection_](https://beam.apache.org/documentation/basics/#pcollection) - A `PCollection` is a data set or data stream. The data that a pipeline processes is part of a PCollection.  
    PCollection - `PCollection` 是数据集或数据流。管道处理的数据是 PCollection 的一部分。
- [_PTransform_](https://beam.apache.org/documentation/basics/#ptransform) - A `PTransform` (or _transform_) represents a data processing operation, or a step, in your pipeline. A transform is applied to zero or more `PCollection` objects, and produces zero or more `PCollection` objects.  
    PTransform - `PTransform` （或转换）表示管道中的数据处理操作或步骤。变换应用于零个或多个 `PCollection` 对象，并生成零个或多个 `PCollection` 对象。
- [_Aggregation_](https://beam.apache.org/documentation/basics/#aggregation) - Aggregation is computing a value from multiple (1 or more) input elements.  
    聚合 - 聚合是根据多个（1 个或多个）输入元素计算值。
- [_User-defined function (UDF)_](https://beam.apache.org/documentation/basics/#user-defined-function-udf) - Some Beam operations allow you to run user-defined code as a way to configure the transform.  
    用户定义函数 (UDF) - 某些 Beam 操作允许运行用户定义代码作为配置转换的一种方式。
- [_Schema_](https://beam.apache.org/documentation/basics/#schema) - A schema is a language-independent type definition for a `PCollection`. The schema for a `PCollection` defines elements of that `PCollection` as an ordered list of named fields.  
    架构 - 架构是 `PCollection` 的独立于语言的类型定义。 `PCollection` 的架构将 `PCollection` 的元素定义为命名字段的有序列表。
- [_SDK_](https://beam.apache.org/documentation/sdks/java/) - A language-specific library that lets pipeline authors build transforms, construct their pipelines, and submit them to a runner.  
    SDK - 特定于语言的库，可让管道作者构建转换、构造管道并将其提交给运行器。
- [_Runner_](https://beam.apache.org/documentation/basics/#runner) - A runner runs a Beam pipeline using the capabilities of your chosen data processing engine.  
    Runner - Runner 使用选择的数据处理引擎的功能运行 Beam 管道。
- [_Window_](https://beam.apache.org/documentation/basics/#window) - A `PCollection` can be subdivided into windows based on the timestamps of the individual elements. Windows enable grouping operations over collections that grow over time by dividing the collection into windows of finite collections.  
    窗口 - `PCollection` 可以根据各个元素的时间戳细分为窗口。 Windows 通过将集合划分为有限集合的窗口，可以对随时间增长的集合进行分组操作。
- [_Watermark_](https://beam.apache.org/documentation/basics/#watermark) - A watermark is a guess as to when all data in a certain window is expected to have arrived. This is needed because data isn’t always guaranteed to arrive in a pipeline in time order, or to always arrive at predictable intervals.  
    水位线 - 水位线是对某个窗口中的所有数据预计何时到达的猜测。这是必要的，因为并不总是保证数据按时间顺序到达管道，或者总是以可预测的时间间隔到达。
- [_Trigger_](https://beam.apache.org/documentation/basics/#trigger) - A trigger determines when to aggregate the results of each window.  
    触发器 - 触发器确定何时聚合每个窗口的结果。
- [_State and timers_](https://beam.apache.org/documentation/basics/#state-and-timers) - Per-key state and timer callbacks are lower level primitives that give you full control over aggregating input collections that grow over time.  
    状态和计时器 - 每个键状态和计时器回调是较低级别的原语，使得可以完全控制随着时间的推移而增长的聚合输入集合。
- [_Splittable DoFn_](https://beam.apache.org/documentation/basics/#splittable-dofn) - Splittable DoFns let you process elements in a non-monolithic way. You can checkpoint the processing of an element, and the runner can split the remaining work to yield additional parallelism.  
    可拆分 DoFn - 可拆分 DoFn 允许以非整体方式处理元素。可以检查元素的处理，并且运行程序可以分割剩余的工作以产生额外的并行性。