---
tags:
  - bigdata
  - beam
---
## Overview
### Element-wise 逐元素

|Transform 转换|Description 描述(描述)|
|---|---|
|[Filter  筛选](https://beam.apache.org/documentation/transforms/java/elementwise/filter)|Given a predicate, filter out all elements that don't satisfy the predicate.  <br>给定一个谓词，过滤掉所有不满足该谓词的元素。|
|[FlatMapElements  平面展开元素](https://beam.apache.org/documentation/transforms/java/elementwise/flatmapelements)|Applies a function that returns a collection to every element in the input and outputs all resulting elements.  <br>应用一个函数，该函数将集合返回到输入中的每个元素并输出所有结果元素。|
|[Keys  按键](https://beam.apache.org/documentation/transforms/java/elementwise/keys)|Extracts the key from each element in a collection of key-value pairs.  <br>从键值对集合中的每个元素中提取键。|
|[KvSwap  Kv交换](https://beam.apache.org/documentation/transforms/java/elementwise/kvswap)|Swaps(交换) the key and value of each element in a collection of key-value pairs.  <br>交换键值对集合中每个元素的键和值。|
|[MapElements  映射为元素](https://beam.apache.org/documentation/transforms/java/elementwise/mapelements)|Applies a function to every element in the input and outputs the result.  <br>将函数应用于输入中的每个元素并输出结果。|
|[ParDo  帕多](https://beam.apache.org/documentation/transforms/java/elementwise/pardo)|The most-general mechanism(机制) for applying a user-defined `DoFn` to every element in the input collection.  <br>将用户定义的 `DoFn` 应用到输入集合中的每个元素的最通用机制。|
|[Partition  分割](https://beam.apache.org/documentation/transforms/java/elementwise/partition)|Routes each input element to a specific output collection based on some partition function.  <br>根据某些分区函数将每个输入元素路由到特定的输出集合。|
|[Regex  正则表达式](https://beam.apache.org/documentation/transforms/java/elementwise/regex)|Filters input string elements based on a regex. May also transform them based on the matching groups.  <br>基于正则表达式过滤输入字符串元素。还可以根据匹配组对它们进行转换。|
|[Reify  具体化](https://beam.apache.org/documentation/transforms/java/elementwise/reify)|Transforms for converting between explicit(显式) and implicit(隐式) form of various Beam values.  <br>用于在各种 Beam 值的显式和隐式形式之间进行转换的变换。|
|[ToString  转字符串](https://beam.apache.org/documentation/transforms/java/elementwise/tostring)|Transforms every element in an input collection to a string.  <br>将输入集合中的每个元素转换为字符串。|
|[WithKeys  带Key](https://beam.apache.org/documentation/transforms/java/elementwise/withkeys)|Produces a collection containing each element from the input collection converted to a key-value pair, with a key selected by applying a function to the input element.  <br>生成一个集合，其中包含转换为键值对的输入集合中的每个元素，并通过将函数应用于输入元素来选择键。|
|[WithTimestamps  带时间戳](https://beam.apache.org/documentation/transforms/java/elementwise/withtimestamps)|Applies a function to determine a timestamp to each element in the output collection, and updates the implicit(隐式) timestamp associated(关联) with each input. Note that it is only safe to adjust timestamps forwards.  <br>应用函数来确定输出集合中每个元素的时间戳，并更新与每个输入关联的隐式时间戳。请注意，只有向前调整时间戳才是安全的。|
|[Values  值](https://beam.apache.org/documentation/transforms/java/elementwise/values)|Extracts the value from each element in a collection of key-value pairs.  <br>从键值对集合中的每个元素中提取值。|

### Aggregation 聚合

|Transform 转换|Description 描述(描述)|
|---|---|
|[ApproximateQuantiles  近似分位数](https://beam.apache.org/documentation/transforms/java/aggregation/approximatequantiles)|Uses an approximation algorithm to estimate the data distribution(分布) within each aggregation using a specified number of quantiles.  <br>使用近似算法通过指定数量的分位数来估计每个聚合内的数据分布。|
|[ApproximateUnique  近似唯一](https://beam.apache.org/documentation/transforms/java/aggregation/approximateunique)|Uses an approximation algorithm to estimate the number of unique elements within each aggregation.  <br>使用近似算法来估计每个聚合中唯一元素的数量。|
|[CoGroupByKey  按键共同分组](https://beam.apache.org/documentation/transforms/java/aggregation/cogroupbykey/)|Similar to `GroupByKey`, but groups values associated(关联) with each key into a batch of a given size  <br>与 `GroupByKey` 类似，但将与每个键关联的值分组到给定大小的批次中|
|[Combine  结合](https://beam.apache.org/documentation/transforms/java/aggregation/combine)|Transforms to combine elements according to a provided `CombineFn`.  <br>根据提供的 `CombineFn` 转换为组合元素。|
|[CombineWithContext  与上下文结合](https://beam.apache.org/documentation/transforms/java/aggregation/combinewithcontext)|An extended version of Combine which allows accessing side-inputs and other context.  <br>组合的扩展版本，允许访问侧面输入和其他上下文。|
|[Count  数数](https://beam.apache.org/documentation/transforms/java/aggregation/count)|Counts the number of elements within each aggregation.  <br>计算每个聚合中的元素数量。|
|[Distinct 去重](https://beam.apache.org/documentation/transforms/java/aggregation/distinct)|Produces a collection containing distinct(鲜明) elements from the input collection.  <br>生成一个包含来自输​​入集合的不同元素的集合。|
|[GroupByKey  按键分组](https://beam.apache.org/documentation/transforms/java/aggregation/groupbykey)|Takes a keyed collection of elements and produces a collection where each element consists of a key and all values associated(关联) with that key.  <br>获取带键的元素集合并生成一个集合，其中每个元素都包含一个键以及与该键关联的所有值。|
|[GroupIntoBatches  分组](https://beam.apache.org/documentation/transforms/java/aggregation/groupintobatches)|Batches values associated(关联) with keys into `Iterable` batches of some size. Each batch contains elements associated(关联) with a specific key.  <br>将与键关联的值批量分成一定大小的 `Iterable` 批次。每个批次包含与特定键关联的元素。|
|[HllCount  希尔计数](https://beam.apache.org/documentation/transforms/java/aggregation/hllcount)|Estimates the number of distinct(鲜明) elements and creates re-aggregatable sketches(素描) using the HyperLogLog++ algorithm.  <br>估计不同元素的数量并使用 HyperLogLog++ 算法创建可重新聚合的草图。|
|[Latest  最新的](https://beam.apache.org/documentation/transforms/java/aggregation/latest)|Selects the latest element within each aggregation according to the implicit(隐式) timestamp.  <br>根据隐式时间戳选择每个聚合中的最新元素。|
|[Max  最大限度](https://beam.apache.org/documentation/transforms/java/aggregation/max)|Outputs the maximum element within each aggregation.  <br>输出每个聚合内的最大元素。|
|[Mean  意思是](https://beam.apache.org/documentation/transforms/java/aggregation/mean)|Computes the average within each aggregation.  <br>计算每个聚合内的平均值。|
|[Min  最小](https://beam.apache.org/documentation/transforms/java/aggregation/min)|Outputs the minimum element within each aggregation.  <br>输出每个聚合中的最小元素。|
|[Sample  样本](https://beam.apache.org/documentation/transforms/java/aggregation/sample)|Randomly(随机) select some number of elements from each aggregation.  <br>从每个聚合中随机选择一些元素。|
|[Sum  和](https://beam.apache.org/documentation/transforms/java/aggregation/sum)|Compute the sum of elements in each aggregation.  <br>计算每个聚合中元素的总和。|
|[Top  顶部](https://beam.apache.org/documentation/transforms/java/aggregation/top)|Compute the largest element(s) in each aggregation.  <br>计算每个聚合中的最大元素。|

### Other 其他

|Transform 转换|Description 描述(描述)|
|---|---|
|[Create  创造](https://beam.apache.org/documentation/transforms/java/other/create)|Creates a collection from an in-memory list.  <br>从内存中的列表创建集合。|
|[Flatten 展平(扁平化)](https://beam.apache.org/documentation/transforms/java/other/flatten)|Given multiple input collections, produces a single output collection containing all elements from all of the input collections.  <br>给定多个输入集合，生成一个包含所有输入集合中的所有元素的输出集合。|
|[PAssert  断言](https://beam.apache.org/documentation/transforms/java/other/passert)|A transform to assert(断言) the contents of a `PCollection` used as part of testing a pipeline either locally or with a runner.  <br>用于断言 `PCollection` 内容的转换，用作本地或运行程序测试管道的一部分。|
|[View  看法](https://beam.apache.org/documentation/transforms/java/other/view)|Operations for turning a collection into view that may be used as a side-input to a `ParDo`.  <br>将集合转换为可用作 `ParDo` 侧输入的视图的操作。|
|[Window  窗户](https://beam.apache.org/documentation/transforms/java/other/window)|Logically(逻辑) divides up or groups the elements of a collection into finite(有限) windows according to a provided `WindowFn`.  <br>根据提供的 `WindowFn` 将集合的元素逻辑地划分或分组到有限窗口中。|


*大道万千，变换无穷*
## Pipelien Design
serial
![[Pasted image 20231009142744.png]]
exmpale1 过滤模式（Filter Pattern）![[Pasted image 20231009143605.png]]

Multi Output
![[Pasted image 20231009142753.png]]
exmpale1 分离模式（Splitter Pattern)![[Pasted image 20231009143610.png]]
```java
// 首先定义每一个output的tag
final TupleTag<User> fiveStarMembershipTag = new TupleTag<User>(){};
final TupleTag<User> goldenMembershipTag = new TupleTag<User>(){};
final TupleTag<User> diamondMembershipTag = new TupleTag<User>(){};

PCollection<User> userCollection = ...;

PCollectionTuple mixedCollection =
    userCollection.apply(ParDo
        .of(new DoFn<User, User>() {
          @ProcessElement
          public void processElement(ProcessContext c) {
            if (isFiveStartMember(c.element())) {
              c.output(c.element());
            } else if (isGoldenMember(c.element())) {
              c.output(goldenMembershipTag, c.element());
            } else if (isDiamondMember(c.element())) {
    c.output(diamondMembershipTag, c.element());
  }
          }
        })
        .withOutputTags(fiveStarMembershipTag,
                        TupleTagList.of(goldenMembershipTag).and(diamondMembershipTag)));

// 分离出不同的用户群组
mixedCollection.get(fiveStarMembershipTag).apply(...);

mixedCollection.get(goldenMembershipTag).apply(...);

mixedCollection.get(diamondMembershipTag).apply(...);
```

Mutli Input or Side Input
example1 合并模式（Joiner Pattern)
![[Pasted image 20231009150656.png]]
```java
PCollectionList<Image> collectionList = PCollectionList.of(internalImages).and(thirdPartyImages);
PCollection<Image> mergedCollectionWithFlatten = collectionList
    .apply(Flatten.<Image>pCollections());

mergedCollectionWithFlatten.apply(...);
```
## Transforms
### Core
- ParDo（通用并行处理）：允许自定义函数对每个输入元素进行处理，并生成零个或多个输出元素
-  GroupByKey（按 Key 分组）：将具有相同 key 的元素分组在一起
- GoGroupByKey（按 Key 合并）：将具有相同 key 的多个输入集合合并为一个 kv集合
- Combine（合并）：通常用于对每个键的值进行聚合操作，例如计算总和、平均值、最大值等。
- Flatten（扁平化）：将多个输入集合合并为单个输出集合。适用于合并多个数据源的情况
- Partition（分区）：将数据集按照指定的规则分成多个分区.
#### ParDo（并行处理）
ParDo 是一个通用的 Transform 类型，它允许自定义函数对每个输入元素进行处理，并生成零个或多个输出元素。
通过 ParDo，可以实现更复杂的数据处理逻辑。例如，将数字平方并过滤出大于10的结果：

```java
PCollection<Integer> numbers = ...;
PCollection<Integer> squaredAndFiltered = numbers.apply(ParDo.of(new DoFn<Integer, Integer>() {
    @ProcessElement
    public void processElement(ProcessContext c) {
        int number = c.element();
        int squared = number * number;
        if (squared > 10) {
            c.output(squared);
        }
    }
}));
```
#### GroupByKey（按键分组）
将具有相同键的元素分组在一起。例如，按键对数字进行分组：
```java
PCollection<KV<String, Integer>> data = ...;
PCollection<KV<String, Iterable<Integer>>> groupedData = data.apply(GroupByKey.create());
```

#### CoGroupByKey（按键合并）
将具有相同键的多个输入集合合并为一个键值对集合。适用于多路合并和联结操作。例如，合并两个键值对集合：
```java
PCollection<KV<String, Integer>> input1 = ...;
PCollection<KV<String, String>> input2 = ...;
PCollection<KV<String, CoGbkResult>> coGrouped = KeyedPCollectionTuple.of(input1, input2)
    .apply(CoGroupByKey.create());
```
#### Combine（合并）
Combine  通常用于对每个键的值进行聚合操作，例如计算总和、平均值、最大值等。它可以在并行处理中高效地对数据进行聚合，而无需显式地进行全局排序或全局收集。

组合函数应该是交换式和关联式的，因为函数并不一定要对具有给定键的所有值精确调用一次。由于输入数据（包括值集合）可能分布在多个 Worker 上，因此组合函数可能会被多次调用，以便对值集合的子集执行部分组合。

Beam SDK 还提供了一些预构建的组合函数，用于常用的数字组合操作，如求和、最小值和最大值。

简单的组合操作（如求和）通常可以用一个简单的函数来实现。更复杂的组合操作可能需要创建 CombineFn 的子类，该子类的累加类型有别于输入/输出类型。

Combine Transform 需要指定一个 CombineFn，用于指定如何对输入集合的元素进行合并和聚合。CombineFn 是一个抽象类，需要自定义实现。在实现 CombineFn 时，需要定义以下四个方法：

1. `createAccumulator()`: 创建并返回一个新的累加器对象，用于保存聚合的中间结果。
2. `addInput(accumulator, input)`: 将输入元素 input 合并到累加器 accumulator 中，并返回更新后的累加器。
3. `mergeAccumulators(accumulators)`: 将多个累加器 accumulators 进行合并，并返回一个合并后的累加器。
4. `extractOutput(accumulator)`: 从累加器 accumulator 中提取最终的聚合结果，并返回。
#### Combining a PCollection into a single value
##### Combine.globally
将一组元素合并为一个单一的结果。例如，计算数字的总和：
```java
PCollection<Integer> numbers = ...;
PCollection<Integer> sum = numbers.apply(Combine.globally(Sum.ofIntegers()));
```
##### CombinePerKey（按键合并）
对具有相同键的元素进行聚合。适用于对每个键进行独立的聚合操作。例如，计算每个键对应的平均值：

```java
PCollection<KV<String, Integer>> data = ...;
PCollection<KV<String, Double>> averagePerKey = data.apply(Combine.perKey(new AverageFn()));
```
其中，`AverageFn` 是自定义的聚合函数，用于计算平均值。

##### Combine and non-global windowing

如果您的 PCollection 使用任何非全局窗口功能，Beam 不会提供默认行为。应用 Combine 时必须指定以下选项之一：
- 指定 .withoutDefaults，即输入 PCollection 中为空的窗口在**输出集合中同样为空**。
- 指定 .asSingletonView，在这种情况下，输出将立即转换为 PCollectionView，在作为侧输入使用时，它将为每个空窗口提供默认值。一般来说，只有当管道组合的结果将在管道的稍后部分用作侧输入时，才需要使用该选项。

#### Flatten（扁平化）
将多个输入集合合并为单个输出集合。适用于合并多个数据源的情况。例如，合并两个整数集合：
```java
PCollection<Integer> collection1 = ...;
PCollection<Integer> collection2 = ...;
PCollection<Integer> mergedCollection = PCollectionList.of(collection1).and(collection2).apply(Flatten.pCollections());
```
#### Partition（分区）
将数据集按照指定的规则分成多个分区。例如，将数字分为奇数和偶数两个分区：
```java
PCollection<Integer> numbers = ...;
TupleTag<Integer> evenTag = new TupleTag<>("even");
TupleTag<Integer> oddTag = new TupleTag<>("odd");
PCollectionTuple result = numbers.apply(Partition.of(2, number -> number % 2 == 0 ? evenTag : oddTag));
PCollection<Integer> evenNumbers = result.get(evenTag);
PCollection<Integer> oddNumbers = result.get(oddTag);
```
##### Reshuffle（重分区）
重新分区数据集，可用于优化数据分发和并行处理。例如，对数据进行随机重分区：
```java
PCollection<Integer> numbers = ...;
PCollection<Integer> reshuffledNumbers = numbers.apply(Reshuffle.viaRandomKey());
```
### Side Input
- [Side Input Architecture for Apache Beam](https://docs.google.com/document/d/1lCIPzkopkEcRKBOvc7lQFWUAmQmRBqTbnT0v-KKtiC8/edit#heading=h.vsm77zumek69).
- [Side input patterns](https://beam.apache.org/documentation/patterns/side-inputs/).
除了主输入 PCollection 外，还可以通过 Side input 的形式为 ParDo Transform 提供额外的Input。

Side Input 是 DoFn 每次处理输入 PCollection 中的元素时可以访问的额外输入。当指定一个 Side Input  时，就创建了一个其他数据视图，可以在处理每个元素时从 ParDo 变换的 DoFn 中读取。

如果 ParDo 需要在处理输入 PCollection 中的每个元素时注入附加数据，但**附加数据需要在运行时确定**（而不是硬编码），那么 Side Input  就非常有用。这些值可能由输入数据决定，也可能取决于 Pipeline 的不同分支。

>PCollectionView\<T> 是 PCollection 的不可变视图，作为类型 T 的值，可以作为 ParDo 转换的Side Input(侧面输入)进行访问。
>
>PCollectionView 应该始终是 org.apache.beam.sdk.transforms.PTransform 的输出。 此转换和每个 PipelineRunner 共同负责以特定于运行器的方式实现视图。
>
>最常见的情况是使用视图转换来准备 PCollection 用作 ParDo 的侧面输入。 有关 SDK 中可用的特定视图的更多详细信息，请参阅 [[View.asSingleton]]()、[[View.asIterable]]() 和 [[View.asMap]]()。
>
>注意：视图方法不应被视为用户可访问。 它们是 Runner 如何获取构建视图所需信息的实现细节，并且可能随时更改。

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
#### Side input and windowing
窗口化 PCollection 可能是无限的，因此无法压缩成单个值（或单个集合类）。当为窗口 PCollection 创建 PCollectionView 时，PCollectionView 代表每个窗口的单个实体（每个窗口一个单例、每个窗口一个列表等）。

Beam **使用主输入元素的窗口来查找侧输入元素的相应窗口。** Beam 会将主输入元素的窗口投射到侧输入元素的窗口集，然后从生成的窗口中使用侧输入元素。如果主输入和侧输入的窗口完全相同，则投影会提供完全对应的窗口。但是，如果输入窗口不同，Beam 会使用投影来选择最合适的侧输入窗口。

**例如，如果主输入使用一分钟的固定时间窗口，而侧输入使用一小时的固定时间窗口，Beam 会根据侧输入窗口集投射主输入窗口，并从适当的一小时侧输入窗口中选择侧输入值。**

如果主输入元素存在于多个窗口中，那么 processElement 会被多次调用，每个窗口调用一次。每次调用 processElement 都会投射主输入元素的 "当前 "窗口，因此每次都可能提供不同的侧输入视图。

如果侧输入有多次触发，Beam 会使用最近一次触发的值。如果使用带有单个全局窗口的侧输入并指定了 Trigger，这一点尤其有用。
#### Side input Case resolve
[[Beam Transform Sideinput]] 
### Additional outputs
#### Split（分割）
Split Transform 用于将数据集按照指定的条件分割成多个数据集。
它接受一个 PCollection 作为输入，并返回一个包含多个 PCollection 的 PCollectionTuple，每个 PCollection 对应一个满足条件的子集。

例如：将输入的整数数据集按照奇偶性分割成两个子集，一个包含偶数，一个包含奇数。Split Transform 返回一个 PCollectionTuple，可以通过 `get()` 方法获取指定标签的子集。
```java
PCollection<Integer> input = ...;
TupleTag<Integer> evenTag = new TupleTag<>("even");
TupleTag<Integer> oddTag = new TupleTag<>("odd");
PCollectionTuple result = input.apply(Split.<Integer>into(evenTag, oddTag)
    .with((Integer number) -> number % 2 == 0 ? evenTag : oddTag));
PCollection<Integer> evenNumbers = result.get(evenTag);
PCollection<Integer> oddNumbers = result.get(oddTag);
```
### Window（窗口）
[[Beam Window]] 将数据集划分为不同的时间窗口，以支持基于事件时间或处理时间的操作。例如，将数据按固定时间窗口划分：
```java
PCollection<Integer> numbers = ...;
PCollection<Integer> windowedNumbers = numbers.apply(Window.into(FixedWindows.of(Duration.standardMinutes(5))));
```

除了固定时间窗口外，还有其他类型的窗口可用，如滑动窗口、会话窗口等。窗口可以根据事件时间或处理时间对数据进行分组和聚合操作。例如，使用滑动窗口对数据进行聚合：

```java
PCollection<Integer> numbers = ...;
PCollection<Integer> sumPerWindow = numbers.apply(Window.into(SlidingWindows.of(Duration.standardMinutes(10))
    .every(Duration.standardMinutes(5))))
    .apply(Sum.integersGlobally());
```
#### Watermark
Watermark（水位线）是一种用于处理事件时间（event time）的概念。**事件时间是指数据在源系统中产生的时间**，而不是数据进入 Apache Beam 流水线的时间。
>Watermark 是一个衡量事件时间进展的标记，表示在给定时间点之前的所有事件都已到达系统。

Watermark 的存在是为了处理延迟数据和乱序数据的情况，以确保正确的窗口计算和时间窗口的关闭。
### Other Transforms
#### Count（计数）
Count Transform 用于计算数据集中的元素数量。
它接受一个 PCollection 作为输入，并返回一个包含单个元素的 PCollection，该元素是输入集合中的元素数量。

例如：对输入的字符串数据集进行计数，并返回一个包含单个 Long 类型元素的 PCollection，该元素表示输入数据集中的元素数量。
```java
PCollection<String> input = ...;
PCollection<Long> count = input.apply(Count.globally());
```
更多关于 Count 参考下面的 『疑惑解析-关于Count』
#### Top（取前N个元素）
从数据集中选择最大或最小的 N 个元素。例如，选择前10个最大的数字：

```java
PCollection<Integer> numbers = ...;
PCollection<List<Integer>> topNumbers = numbers.apply(Top.of(10, Order.byValue()));
```
#### Filter（过滤）
根据给定的条件筛选元素。例如，筛选出偶数：
```java
PCollection<Integer> numbers = ...;
PCollection<Integer> evenNumbers = numbers.apply(Filter.by((Integer number) -> number % 2 == 0));
```
#### Distinct（去重）
去除数据集中的重复元素。例如，去除重复的字符串：
```java
PCollection<String> strings = ...;
PCollection<String> distinctStrings = strings.apply(Distinct.create());
```
#### Map（映射）
对每个输入元素应用一个函数，并生成一个新的元素作为输出。例如，将每个数字乘以2：
```java
PCollection<Integer> numbers = ...;
PCollection<Integer> multipliedNumbers = numbers.apply(MapElements.into(TypeDescriptors.integers())
    .via((Integer number) -> number * 2));
```
#### FlatMap（扁平映射）
对每个输入元素应用一个函数，并生成零个或多个输出元素。例如，将句子拆分为单词：
```java
PCollection<String> sentences = ...;
PCollection<String> words = sentences.apply(FlatMapElements.into(TypeDescriptors.strings())
    .via((String sentence) -> Arrays.asList(sentence.split(" "))));
```
#### Join（连接）
将具有相同键的两个输入集合进行连接操作。例如，连接两个键值对集合：

```java
PCollection<KV<String, Integer>> input1 = ...;
PCollection<KV<String, String>> input2 = ...;
PCollection<KV<String, CoGbkResult>> joinedData = KeyedPCollectionTuple.of(input1, input2)
    .apply(CoGroupByKey.create())
    .apply(ParDo.of(new DoFn<KV<String, CoGbkResult>, KV<String, String>>() {
        @ProcessElement
        public void processElement(ProcessContext c) {
            KV<String, CoGbkResult> element = c.element();
            String key = element.getKey();
            Iterable<Integer> values1 = element.getValue().getAll(input1);
            Iterable<String> values2 = element.getValue().getAll(input2);
            // Process the joined values
            ...
            c.output(KV.of(key, ...));
        }
    }));
```
## Transform internal
[[Beam PCollection]] and Bundle
example1 
![[Pasted image 20231009142821.png]]
example2
![[Pasted image 20231009142904.png]]

Worker and Bundle
exmaple1![[Pasted image 20231009142836.png]]
example2![[Pasted image 20231009142918.png]]

PTransform and Bundle
![[Pasted image 20231009142935.png]]

## 疑惑解析
### GroupbyKey and Partition
对于groupByKey和partition两个操作：

groupByKey 是指将数据根据某个键或键组合进行分组。通过分组，具有相同键或键组合的数据被聚合在一起，以便进行后续的处理。
	例如，可以根据用户ID将数据分组，以便对每个用户的数据进行汇总或聚合操作。

Partition 是指将数据分割成多个独立的数据集，每个数据集称为一个分区。
分区可以按照某个键或哈希函数对数据进行划分，使得每个分区中的数据在某种程度上是相互独立的。分区通常用于并行处理数据，以提高处理性能。
	例如，可以根据数据的时间戳将数据分区，使得每个分区包含一段时间范围内的数据。

groupByKey and Partition 的比较：
1. 作用方面
- groupByKey用于根据指定的key对PCollection进行分组。它会将具有相同key的元素分到同一个组中。
- partition用于将PCollection重新分区。它允许自定义分区逻辑,控制元素输出到哪个分区。

2. 区别
- groupByKey是按照元素的key进行分组,而partition可以自定义分区逻辑,更加灵活。
- groupByKey producr出的PCollection中每个元素是一个KV对,其中Key是唯一的,Value是这个Key对应的所有元素。partition产生的PCollection结构不变。
- groupByKey可以在一个节点内完成操作,而partition需要在多个节点间重分配数据。

3. 使用场景
- groupByKey常用于分组聚合统计,比如计算每个key对应的平均值等。
- partition常用于重分区,将数据按新的逻辑重新分配到不同的节点上,可实现负载均衡。

groupByKey是按key分组,partition是重新分区。groupByKey用在分组聚合统计,partition用在重分区和负载均衡。

### OutputWithTag 与 Partition 和 GroupByKey
`OutputWithTag`与`Partition`和`GroupByKey`有一些相同和不同之处。下面是它们之间的比较：

相同之处：
1. 数据分流：`OutputWithTag`、`Partition`和`GroupByKey`都涉及到将数据拆分为不同的组或分片。
2. 键值对操作：`OutputWithTag`和`GroupByKey`操作都基于键值对（key-value pairs）进行操作。

区别之处：
1. 功能和目的：
   - `OutputWithTag`：它是一种特殊的输出操作，允许您为每个输出元素附加一个标签，并将它们发送到不同的输出通道。这对于将数据分发到不同的目标（例如不同的文件、不同的目录等）非常有用。
   - `Partition`：它是一种数据分片操作，将输入数据分成多个分片，每个分片可以在不同的`PCollection`中进行处理。通常用于将数据划分为更小的数据集，以便进行并行处理。
   - `GroupByKey`：它是一种将具有相同键的元素分组在一起的操作，以便进行聚合或其他处理。它会将具有相同键的输入元素合并为一个键值对，并生成一个包含聚合结果的新`PCollection`。

2. 输入和输出类型：
   - `OutputWithTag`：它接收一个输入元素，并将其输出到一个或多个具有标签的输出通道。
   - `Partition`：它接收一个`PCollection`作为输入，并将其分成多个分片，每个分片都是一个独立的`PCollection`。
   - `GroupByKey`：它接收一个键值对的输入`PCollection`，并将具有相同键的元素分组在一起生成新的`PCollection`。

1. 使用情况：
   - `OutputWithTag`：适用于将数据路由到不同的输出目标，例如将数据写入不同的文件或位置。
   - `Partition`：适用于将数据分片以进行并行处理，可以将数据分发到不同的工作节点或处理器上。
   - `GroupByKey`：适用于按键对数据进行分组和聚合，例如在MapReduce中进行reduce操作。

### 关于 Count
#### perElement
在Apache Beam中，`Count.perElement()`是一种转换操作，用于计算`PCollection`中每个元素的出现次数。它返回一个新的`PCollection`，其中每个元素都是一个键值对，键表示原始元素，值表示该元素在输入`PCollection`中出现的次数。

在进行计数操作时，`Count.perElement()`会对输入`PCollection`中的每个元素进行遍历。它使用哈希表（hash table）或类似的数据结构来跟踪每个元素的出现次数。当遍历到一个新元素时，它将该元素添加到哈希表中，并将计数初始化为1。如果遍历到一个已经存在于哈希表中的元素，则将该元素的计数增加1。
	具体来说，Beam使用键对象的equals()方法来比较两个键是否相等。

该操作在并行处理的环境中也能正常工作。在分布式计算框架中，例如Apache Beam运行在分布式的数据处理引擎上，每个工作节点会处理输入数据的一个子集。在这种情况下，`Count.perElement()`会在每个工作节点上独立计算元素的出现次数，并将结果进行合并以得到最终的计数结果。

需要注意的是，`Count.perElement()`操作返回的`PCollection`中的元素顺序是不确定的，因为它是并行计算的结果。如果需要按照特定的顺序对结果进行处理，可以使用其他的排序或排序相关的操作。

总结起来，`Count.perElement()`通过遍历输入`PCollection`的元素并使用哈希表等数据结构进行计数，实现了对每个元素的出现次数的计算。
#### 自定义 Count
如果想要自定义计数逻辑，可以使用`Combine`转换来实现自定义的计数操作。`Combine`允许定义组合函数，用于聚合`PCollection`中的元素。

示例：
```java
import org.apache.beam.sdk.transforms.Combine;
import org.apache.beam.sdk.transforms.SerializableFunction;

// 自定义计数逻辑
public class MyCountFn extends Combine.CombineFn<String, Long, Long> {
  @Override
  public Long createAccumulator() {
    return 0L; // 初始累加器为0
  }

  @Override
  public Long addInput(Long accumulator, String input) {
    return accumulator + 1; // 每次输入元素时累加1
  }

  @Override
  public Long mergeAccumulators(Iterable<Long> accumulators) {
    long total = 0;
    for (Long accumulator : accumulators) {
      total += accumulator; // 合并累加器
    }
    return total;
  }

  @Override
  public Long extractOutput(Long accumulator) {
    return accumulator; // 返回最终计数结果
  }
}

// 使用自定义计数逻辑
PCollection<String> input = ...; // 输入PCollection
PCollection<Long> counts = input.apply(Combine.globally(new MyCountFn()));

// 在counts中，每个元素表示输入PCollection中的元素总数
```
- `createAccumulator()`：创建初始累加器，默认为0。
- `addInput()`：每次输入一个元素时，累加器加1。
- `mergeAccumulators()`：合并多个累加器，将它们的值相加。
- `extractOutput()`：从最终累加器中提取计数结果。
将自定义的计数逻辑应用于输入的`PCollection`，使用`Combine.globally()`将计数应用于全局范围，生成一个包含计数结果的新的`PCollection`。

## Testing
See Testing Guide here：[Test Your Pipeline](https://beam.apache.org/documentation/pipelines/test-your-pipeline/) .  and [Testing I/O Transforms in Apache Beam](https://beam.apache.org/documentation/io/testing/)

PTransform test example code:
```java
public class PipelineTest {  
    @Rule  
    public final transient TestPipeline pl = TestPipeline.create();  
  		/*  
		## Testing Transforms
		1.Create a TestPipeline.  
		2.Create some static, known test input data.  
		3.Use the Create transform to create a PCollection of your input data.  
		4.Apply your transform to the input PCollection and save the resulting output PCollection.  
		5.Use PAssert and its subclasses to verify that the output PCollection contains the elements that you expect.  
		*/
    @Test  
    public void pipeline() {  
		String[] word_arr = new String[]{  
                "hi", "there", "hi", "hi", "sue", "bob",  
                "hi", "sue", "", "", "ZOW", "bob", ""  
        };  
        List<String> words = Arrays.asList(word_arr);  
        PCollection<String> input = pl.apply(Create.of(words));  
        PCollection<KV<String, Long>> count = input.apply(Count.<String>perElement());  
        // 这里有一点：必须列出所有的结果，比如如果下面这里没有 KV.of("", 3L) 就会报错 not_matched
        PAssert.that(count).containsInAnyOrder(  
                KV.of("hi", 4L),  
                KV.of("there", 1L),  
                KV.of("sue", 2L),  
                KV.of("bob", 2L),  
                KV.of("ZOW", 1L),  
                KV.of("", 3L)  
        );  
        pl.run();  
    }
}
```

**Testing the WordCount Pipeline**
```java
public class WordCountTest {

    // Our static input data, which will comprise the initial PCollection.
    static final String[] WORDS_ARRAY = new String[] {
      "hi there", "hi", "hi sue bob",
      "hi sue", "", "bob hi"};

    static final List<String> WORDS = Arrays.asList(WORDS_ARRAY);

    // Our static output data, which is the expected data that the final PCollection must match.
    static final String[] COUNTS_ARRAY = new String[] {
        "hi: 5", "there: 1", "sue: 2", "bob: 2"};

    // Example test that tests the pipeline's transforms.

    public void testCountWords() throws Exception {
      Pipeline p = TestPipeline.create();

      // Create a PCollection from the WORDS static input data.
      PCollection<String> input = p.apply(Create.of(WORDS));

      // Run ALL the pipeline's transforms (in this case, the CountWords composite transform).
      PCollection<String> output = input.apply(new CountWords());

      // Assert that the output PCollection matches the COUNTS_ARRAY known static output data.
      PAssert.that(output).containsInAnyOrder(COUNTS_ARRAY);

      // Run the pipeline.
      p.run();
    }
}
```

## Refrence
1 [Beam: PTransform Style Guide PTransform 风格指南](https://beam.apache.org/contribute/ptransform-style-guide/).