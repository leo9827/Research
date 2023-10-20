Aggregating computing functions across data sources
多数据源聚合计算

在 [[Beam]] 有如下两种方式，分别对应不同的时机
## Pipeline IO
To read data from disparate sources into a single `PCollection`, read each one independently and then use the [Flatten](https://beam.apache.org/documentation/programming-guide/#flatten) transform to create a single `PCollection`.  
要将不同来源的数据读取到单个 `PCollection` 中，需要独立读取每个数据，然后使用 `Flatten` 转换创建单个 `PCollection` 。
### Flatten
`Flatten` 是一种用于存储相同数据类型的 `PCollection` 对象的 Beam 变换。`Flatten` 可将多个 `PCollection` 对象合并为一个逻辑 PCollection。
下面的示例展示了如何应用 `Flatten transform` 合并多个 `PCollection`
```java
// Flatten takes a PCollectionList of PCollection objects of a given type.
// Returns a single PCollection that contains all of the elements in the PCollection objects in that list.
PCollection<String> pc1 = ...;
PCollection<String> pc2 = ...;
PCollection<String> pc3 = ...;
PCollectionList<String> collections = PCollectionList.of(pc1).and(pc2).and(pc3);

PCollection<String> merged = collections.apply(Flatten.<String>pCollections());
```
#### Data encoding in merged collections
默认情况下，输出 `PCollection` 的 `coder` 与输入 PCollectionList 中第一个 `PCollection` 的 `coder` 相同。不过，输入 `PCollection` 对象可以使用不同的 `coder`，只要它们在所选语言中包含相同的数据类型即可。
#### Merging windowed collections
在使用 `Flatten` 合并已应用窗口策略的 `PCollection` 对象时，所有要合并的 `PCollection` 对象都必须使用兼容的窗口策略和窗口大小。
例如，要合并的所有集合都必须使用（假设）相同的 5 分钟固定窗口或每 30 秒开始的 4 分钟滑动窗口。

如果 `Pipeline` 试图使用 `Flatten` 合并窗口不兼容的 `PCollection` 对象，Beam 会在构建 `Pipeline` 时生成 `IllegalStateException` 错误。
## Transforms
在为 Beam 转换构建用户代码时，应牢记执行的分布式特性。
例如：
- 自定义函数可能有许多副本并行运行在许多不同的机器上，这些副本独立运行，不与任何其他副本通信或共享状态。
- 根据为 `Pipeline` 选择的 `Pipeline Runner` 和处理后端，用户代码函数的每个副本都可能被重试或运行多次。因此，在用户代码中加入状态依赖性等内容时应谨慎。
###  Side inputs 侧面输入
除了主输入 `PCollection` 之外，还可以以`side input`的形式向 `ParDo` 转换提供其他输入。 `Side input` 是 `DoFn` 每次处理输入 `PCollection` 中的元素时可以访问的附加输入。

当指定 `Side input` 时，将创建一些其他数据的视图，在处理每个元素时可以从 `ParDo` 转换的 `DoFn` 中读取这些数据。
>如果 `ParDo` 在处理输入 `PCollection` 中的每个元素时需要注入附加数据，但附加数据需要在运行时确定（而不是硬编码），则侧面输入非常有用。
>这些值可能由输入数据确定，或者取决于管道的不同分支。

#### Passing side inputs to ParDo
```java
  // Pass side inputs to your ParDo transform by invoking .withSideInputs.
  // Inside your DoFn, access the side input by using the method DoFn.ProcessContext.sideInput.

  // The input PCollection to ParDo.
  PCollection<String> words = ...;

  // A PCollection of word lengths that we'll combine into a single value.
  PCollection<Integer> wordLengths = ...; // Singleton PCollection

  // Create a singleton PCollectionView from wordLengths using Combine.globally and View.asSingleton.
  final PCollectionView<Integer> maxWordLengthCutOffView =
     wordLengths.apply(Combine.globally(new Max.MaxIntFn()).asSingletonView());


  // Apply a ParDo that takes maxWordLengthCutOffView as a side input.
  PCollection<String> wordsBelowCutOff =
  words.apply(ParDo
      .of(new DoFn<String, String>() {
          @ProcessElement
          public void processElement(@Element String word, OutputReceiver<String> out, ProcessContext c) {
            // In our DoFn, access the side input.
            int lengthCutOff = c.sideInput(maxWordLengthCutOffView);
            if (word.length() <= lengthCutOff) {
              out.output(word);
            }
          }
      }).withSideInputs(maxWordLengthCutOffView)
  );
```
#### Side inputs and windowing
窗口 `PCollection` 可能是无限的，因此不能压缩为单个值（或单个集合类）。
当创建窗口 `PCollection` 的 `PCollectionView` 时， `PCollectionView` 表示每个窗口一个实体（每个窗口一个单例，每个窗口一个列表等） 。

Beam 使用主输入元素的窗口来查找侧面输入元素的适当窗口。
Beam 将主输入元素的窗口投影到侧输入的窗口集中，然后使用结果窗口中的侧输入。
如果主输入和侧输入具有相同的窗口，则投影提供精确对应的窗口。但是，如果输入具有不同的窗口，Beam 会使用投影来选择最合适的侧面输入窗口。

举例：
如果主输入使用一分钟的固定时间窗口，而 side input 使用一小时的固定时间窗口，则 Beam 会根据 side input 窗口集投射主输入窗口，并从相应的一小时侧输入窗口中选择侧输入值。

如果主输入元素存在于多个窗口中，则 `processElement` 会被多次调用，每个窗口调用一次。每次调用 `processElement` 都会投影主输入元素的“当前”窗口，因此每次可能会提供不同的侧面输入视图。

>如果侧面输入有多个触发器触发，Beam 将使用最近一次触发器触发的值。如果您使用带有单个全局窗口的侧面输入并指定触发器，这尤其有用。
