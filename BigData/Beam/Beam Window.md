---
tags:
  - bigdata
  - beam
---
## Window
在 Apache Beam 中，Window（窗口）是一种用于对数据流进行**分组**和聚合操作的概念。它将数据流划分为不同的时间窗口，使得可以**在每个窗口内对数据进行独立的计算**。

划分依赖于时间。具体来说，数据是否在一个窗口中是根据数据元素的时间戳（Timestamp）和窗口的定义来确定的。

### 添加timestamp
自定义给PCollection中的元素添加timestamp
```java
        // 使用WithTimestamps转换函数为元素添加时间戳
        PCollection<String> linesWithTimestamp = lines.apply(
            WithTimestamps.of((String line) -> {
                // 在此处指定时间戳逻辑，这里使用当前时间作为时间戳
                return Instant.now();
            })
        );
```

其他方式

1.使用`ParDo`转换：您可以将一个`ParDo`转换应用于`PCollection`并在`DoFn`的`ProcessElement`方法中为每个元素手动添加时间戳。例如：

```java
import org.apache.beam.sdk.transforms.DoFn;
import org.apache.beam.sdk.values.PCollection;

PCollection<MyElement> input = ...;

PCollection<MyElement> output = input.apply(ParDo.of(new DoFn<MyElement, MyElement>() {
   @ProcessElement
   public void processElement(ProcessContext c) {
	   MyElement element = c.element();
	   // 添加时间戳逻辑
	   Instant timestamp = ...; // 根据需要计算时间戳
	   c.outputWithTimestamp(element, timestamp);
   }
}));
```

2.使用`MapElements`转换：您可以使用`MapElements`转换，并在`SimpleFunction`或`SerializableFunction`的`apply`方法中为每个元素添加时间戳。例如：

```java
import org.apache.beam.sdk.transforms.MapElements;
import org.apache.beam.sdk.values.TypeDescriptor;

PCollection<MyElement> input = ...;

PCollection<MyElement> output = input.apply(MapElements.into(TypeDescriptor.of(MyElement.class))
   .via((MyElement element) -> {
	   // 添加时间戳逻辑
	   Instant timestamp = ...; // 根据需要计算时间戳
	   return TimestampedValue.of(element, timestamp);
   }));
```

3.Kafka connector 自定义timestamp policy
```java
// pipeline read with kafka connector
		 pipeline
            .apply(KafkaIO.<String, String>read()
                .withBootstrapServers(bootstrapServers)
                .withTopics(Collections.singletonList(topic))
                .withKeyDeserializer(StringDeserializer.class)
                .withValueDeserializer(StringDeserializer.class)
                .updateConsumerProperties(ImmutableMap.of("auto.offset.reset", "earliest"))
                .withoutMetadata()
                .withTimestampPolicyFactory((tp, previousWatermark) -> new CustomTimestampPolicy())
            )
            .apply(ParDo.of(new ProcessMessageFn()));

// custom timestamp policy
import org.apache.beam.sdk.io.kafka.KafkaRecord;
import org.apache.beam.sdk.io.kafka.TimestampPolicy;
import org.apache.kafka.common.TopicPartition;

public class CustomTimestampPolicy extends TimestampPolicy<String, String> {
    @Override
    public Instant getTimestampForRecord(PartitionContext ctx, KafkaRecord<String, String> record) {
        // 根据自定义逻辑确定消息的时间戳
        // 例如，从消息内容中提取时间信息
        Instant timestamp = extractTimestampFromRecord(record);

        return timestamp;
    }

    @Override
    public Instant getWatermark(PartitionContext ctx) {
        // 根据需要确定水位线
        // 例如，基于事件时间窗口计算水位线
        Instant watermark = calculateWatermark(ctx);

        return watermark;
    }

    private Instant extractTimestampFromRecord(KafkaRecord<String, String> record) {
        // 从消息内容中提取时间信息
        // 返回一个合适的Instant对象
    }

    private Instant calculateWatermark(PartitionContext ctx) {
        // 基于事件时间窗口等计算水位线
        // 返回一个合适的Instant对象
    }
}
```

窗口允许对具有相似时间属性的数据进行批处理操作，例如基于时间的聚合、统计、排序等。通过使用窗口，可以处理延迟数据和乱序数据，并控制数据在时间维度上的分组方式
### Fixed Window（固定时间窗口）
将数据流划分为固定长度的连续时间窗口。例如，每个窗口可以是 1 分钟、5 分钟或 1 小时。

例如，将数据按固定时间窗口划分：
```java
PCollection<Integer> numbers = ...;
PCollection<Integer> windowedNumbers = numbers.apply(Window.into(FixedWindows.of(Duration.standardMinutes(5))));
```

除了固定时间窗口外，还有其他类型的窗口可用，如滑动窗口、会话窗口等。窗口可以根据事件时间或处理时间对数据进行分组和聚合操作。
### Sliding Window（滑动时间窗口）
与固定时间窗口类似，但滑动时间窗口可以重叠。

滑动时间窗口有两个参数：窗口长度和滑动步长。
例如，一个滑动时间窗口可以是 1 小时，滑动步长为 10 分钟，这意味着窗口之间有 10 分钟的重叠。

例如，使用滑动窗口对数据进行聚合：
```java
PCollection<Integer> numbers = ...;
PCollection<Integer> sumPerWindow = numbers.apply(Window.into(SlidingWindows.of(Duration.standardMinutes(10))
    .every(Duration.standardMinutes(5))))
    .apply(Sum.integersGlobally());
```
### Session Window（会话时间窗口）
根据数据的活动会话（session）将数据流划分为窗口。会话是指一段连续的活动时间，超过一定的间隔时间后，会话被视为结束。会话时间窗口可以用于基于用户活动或交互的分析场景。

例如，将输入数据集 `events` 应用会话时间窗口（会话间隔为 30 分钟），然后使用 `Count.perKey()` 对每个窗口中的事件按键进行计数。
```java
PCollection<Event> events = ...;

PCollection<KV<String, Long>> sessionCounts = events
    .apply(Window.into(Sessions.withGapDuration(Duration.standardMinutes(30))))
    .apply(Count.perKey());
```
### Global Windows（全局时间窗口）
将整个数据流视为一个窗口，不进行时间分组。全局时间窗口适用于不依赖于时间维度的聚合操作。

例如，将输入数据集 `events` 应用全局时间窗口，然后使用 `Count.perKey()` 对整个数据流进行按键计数。
```java
PCollection<Event> events = ...;

PCollection<KV<String, Long>> globalCounts = events
    .apply(Window.into(new GlobalWindows()))
    .apply(Count.perKey());
```
## Watermark（水位线）
>Watermark 是 Apache Beam 中用于处理事件时间的概念，**它是一个标记**，表示在给定时间点之前的所有事件都已到达系统。
>通过使用 Watermark，可以对数据流进行窗口计算和聚合操作，以处理延迟数据和乱序数据的问题。

Watermark（水位线）是一种用于处理事件时间（event time）的概念。**事件时间是指数据在源系统中产生的时间**，而不是数据进入 Apache Beam 流水线的时间。
>Watermark 是一个衡量事件时间进展的标记，表示在给定时间点之前的所有事件都已到达系统。

Watermark 的存在是为了处理延迟数据和乱序数据的情况，以确保正确的窗口计算和时间窗口的关闭。

在 Apache Beam 中，每个数据元素都可以关联一个时间戳（timestamp），表示该数据元素对应的事件时间。而 **Watermark 表示一个时间戳的下界**，即预计在该时间点之后不会再有比它更早的数据到达。

使用 Watermark，可以在数据流中定义窗口（window），将数据分组并进行聚合操作。当 Watermark 超过某个窗口的结束时间时，该窗口将被触发并进行计算。

例如，假设有一个事件时间流，其中的数据元素包含时间戳信息。Apache Beam 可以根据 Watermark 和窗口定义对数据进行分组和聚合，以便在时间窗口关闭后对数据进行处理。

Watermark 在 Apache Beam 中通过 `WithTimestamps` 或 `TimestampPolicy` 来指定数据元素的时间戳信息，并通过 `Window` Transform 中的窗口定义进行窗口计算和关闭。

举例：
使用 Watermark 需要以下步骤：
1. 为数据元素添加时间戳：首先，需要为数据元素添加时间戳信息，以指示事件时间。这可以通过使用 `WithTimestamps` Transform 或自定义的 `TimestampPolicy` 来完成。`WithTimestamps` Transform 可以将数据元素的时间戳设置为固定的值或从数据元素中提取时间戳字段。如果需要更灵活的时间戳设置逻辑，可以实现自定义的 `TimestampPolicy`。
```java
PCollection<Event> events = ...;

PCollection<Event> eventsWithTimestamps = events.apply(WithTimestamps.of(event -> event.getTimestamp()));
```
上面假设 `Event` 是包含时间戳字段的数据类型。通过 `WithTimestamps` Transform，将数据元素的时间戳设置为 `Event` 对象中的 `timestamp` 字段的值。

2. 定义窗口：接下来，需要定义窗口来对数据进行分组和聚合。窗口定义包括窗口类型（如固定时间窗口、滑动时间窗口等）和窗口的大小、偏移等参数。
```java
PCollection<Event> eventsWithTimestamps = ...;

PCollection<Event> windowedEvents = eventsWithTimestamps.apply(
    Window.into(FixedWindows.of(Duration.standardMinutes(5))));
```
上面使用 `FixedWindows` 来定义固定时间窗口，窗口大小为 5 分钟。`Window.into()` 方法用于将窗口定义应用于数据集。

3. 处理 Window 数据：一旦定义了 Window，就可以使用相应的窗口操作（如 `GroupByKey`、`Combine`、`Reduce` 等）对数据进行聚合操作。
```java
PCollection<Event> windowedEvents = ...;

PCollection<Integer> eventCounts = windowedEvents.apply(Count.perElement());
```
上面使用 `Count.perElement()` 对每个窗口中的事件进行计数。

4. 处理 Window Trigger：当 Watermark 超过窗口的结束时间时，窗口将被触发并进行计算。此时可以使用 `WindowFn` 的 `onWindowExpiration()` 方法来处理窗口触发事件。
```java
PCollection<Event> windowedEvents = ...;

windowedEvents.apply(ParDo.of(new DoFn<Event, Void>() {
  @OnWindowExpiration
  public void onWindowExpiration(BoundedWindow window, OutputReceiver<Void> out) {
    // 处理窗口触发事件
    ...
  }
}));
```
上面使用 `@OnWindowExpiration` 注解来定义处理窗口触发事件的逻辑。

### Watermark Example

当需要处理具有乱序事件或延迟事件的数据流时，自定义 Watermark 可以帮助确定事件时间进展，并在合适的时机触发窗口计算。以下是一个示例，演示如何自定义 Watermark：

假设有一个数据流表示用户行为日志，其中包含用户点击事件的时间戳。基于这些点击事件创建一个窗口，每隔 5 分钟计算一次过去 1 小时内的点击量。由于数据流可能存在乱序或延迟，**希望在数据流中有一定的进展后再触发窗口计算，而不是等待所有事件都到达**。

```java
import org.apache.beam.sdk.Pipeline;
import org.apache.beam.sdk.io.kafka.KafkaIO;
import org.apache.beam.sdk.transforms.windowing.*;
import org.apache.kafka.common.serialization.StringDeserializer;
import org.joda.time.Duration;

public class CustomWatermarkExample {

    public static void main(String[] args) {
        // 创建一个 Pipeline
        Pipeline pipeline = Pipeline.create();

        // 从 Kafka 主题读取用户点击事件日志
        pipeline.apply(KafkaIO.<String, String>read()
                .withBootstrapServers("kafka:9092")
                .withTopic("user-clicks")
                .withKeyDeserializer(StringDeserializer.class)
                .withValueDeserializer(StringDeserializer.class))
                // 提供 TimestampPolicy，为每条记录设置事件时间戳
                .withTimestampsAndWatermarks(new CustomTimestampPolicy())
                // 应用固定窗口，每隔 5 分钟计算过去 1 小时内的点击量
                .apply(Window.into(FixedWindows.of(Duration.standardMinutes(5)))))
                // 计算并输出窗口中的点击量
                .apply("Calculate Clicks", ...);

        // 运行 Pipeline
        pipeline.run();
    }

    static class CustomTimestampPolicy implements KafkaIO.TimestampPolicy<String, String> {
        @Override
        public Instant getTimestampForRecord(PartitionContext ctx, KafkaRecord<String, String> record) {
            // 从记录中获取事件时间戳的逻辑
            // 这里假设记录的值是 JSON 字符串，包含一个名为 "timestamp" 的字段表示事件时间戳
            String jsonValue = record.getKV().getValue();
            long timestamp = extractTimestampFromJson(jsonValue);
            return new Instant(timestamp);
        }

        @Override
        public WatermarkEstimator<Instant> getWatermarkEstimator() {
            return new WatermarkEstimator<Instant>() {
                private Instant currentWatermark = BoundedWindow.TIMESTAMP_MIN_VALUE;

                @Override
                public void observeTimestamp(Instant timestamp) {
                    // 更新水位线的逻辑
                    currentWatermark = timestamp.minus(Duration.standardMinutes(30)); // 设置水位线为事件时间戳减去 30 分钟
                }

                @Override
                public Instant currentWatermark() {
                    // 返回当前水位线
                    return currentWatermark;
                }
            };
        }

        @Override
        public WatermarkPolicy getWatermarkPolicy() {
            // 使用默认的 WatermarkPolicy
            return WatermarkPolicy.withFixedDelay(Duration.standardMinutes(1));
        }
    }

    static long extractTimestampFromJson(String jsonValue) {
        // 从 JSON 字符串中提取事件时间戳的逻辑
        // 这里假设 JSON 字符串的格式为 {"timestamp": 1634446800000}
        JsonObject jsonObject = JsonParser.parseString(jsonValue).getAsJsonObject();
        return jsonObject.get("timestamp").getAsLong();
    }
}
```

在这个示例中从 Kafka 主题中读取用户点击事件日志。通过实现 `TimestampPolicy` 接口，为每条记录设置了事件时间戳。假设事件时间戳存储在记录的值中的 JSON 字段中。

接下来定义了一个自定义的 `WatermarkEstimator`，它根据观察到的事件时间戳来动态更新水位线。在这个示例中，我们简单地将水位线设置为当前事件时间戳减去 30 分钟。这意味着只有当事件时间超过当前时间的 30 分钟时，才会触发窗口计算。

最后用 `WatermarkPolicy.withFixedDelay(Duration.standardMinutes(1))` 创建了一个固定延迟的 WatermarkPolicy。这意味着 Beam 将每隔 1 分钟生成一个 Watermark，表示事件时间的进展。这个 Watermark 将被用于触发窗口计算。

在上述示例中，自定义 Watermark 的作用是根据观察到的事件时间戳来确定数据流的进展，并在一定时间间隔内生成 Watermark。水位线的更新策略可以根据需求进行调整，例如根据实际数据流的延迟情况或业务需求来设置。

通过自定义 Watermark，可以在数据流中有一定进展时触发窗口计算，而不需要等待所有事件都到达。这对于处理乱序事件或存在延迟的数据流非常有用，可以更准确地控制窗口计算的触发时机，并及时输出结果。
