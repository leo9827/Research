## Case resolve
case 1 : `java.lang.IllegalArgumentException: unable to serialize DoFnWithExecutionInformation`
```java
public class SideinputTest {
	@Rule  
	public final transient TestPipeline p = TestPipeline.create();

    @Test  
    public void sideInput() {  
        String[] word_arr = new String[]{  
                "hi", "there", "hi", "hi", "sue", "bob",  
                "hi", "sue", "", "", "ZOW", "bob", ""  
        };  
        List<String> words = Arrays.asList(word_arr);  
  
        PCollection<String> input = p.apply(Create.of(words));  
  
        // create view  
        PCollectionView<Map<String, Long>> inputKeyCount = input  
                .apply(Count.perElement())  
                .apply(View.asMap());  
  
        // process with side input        
        PCollection<KV<String, Long>> output = input
        .apply("after side input count", Count.<String>perElement())
        .apply("process side input", ParDo.of(new DoFn<KV<String, Long>, KV<String, Long>>() {  
                    @ProcessElement  
                    public void process(ProcessContext c) {  
						Map<String, Long> viewMap = c.sideInput(inputKeyCount);  
						Long ct = viewMap.get(c.element().getKey());  
						c.output(KV.of(c.element().getKey(), c.element().getValue() + ct));
                    }  
                }).withSideInputs(inputKeyCount));  
        PAssert.that(output).containsInAnyOrder(  
                KV.of("hi", 8L),  
                KV.of("there", 2L),  
                KV.of("sue", 4L),  
                KV.of("bob", 4L),  
                KV.of("ZOW", 2L),  
                KV.of("", 6L)  
        );  
        p.run();  
    }
}
```
这里会产生Exception：`java.lang.IllegalArgumentException: unable to serialize DoFnWithExecutionInformation{doFn=xxx`。

resolve：
这个问题产生的原因是这里使用了匿名内部类：`ParDo.of(new DoFn<String, String>() {})`, 匿名内部类是无法Serialize（序列化）的，需要将其申明为一个 `class`（静态、非静态均可）显式`implements Serializable` 即可。
另外这里需要在声明的`class`中使用参数`inputKeyCount`，需要申明一个属性用于参数传递
code：
```java
public class SideinputTest {
	@Rule  
	public final transient TestPipeline p = TestPipeline.create();
    static class ProcessSideInputFn extends DoFn<KV<String, Long>, KV<String, Long>> implements Serializable {  
	    private final PCollectionView<Map<String, Long>> inputKeyCount;  
	  
	    public ProcessSideInputFn(PCollectionView<Map<String, Long>> inputKeyCount) {  
	        this.inputKeyCount = inputKeyCount;  
	    }  
	  
	    @ProcessElement  
	    public void process(ProcessContext c) {  
	        Map<String, Long> viewMap = c.sideInput(inputKeyCount);  
	        Long ct = viewMap.get(c.element().getKey());  
	        c.output(KV.of(c.element().getKey(), c.element().getValue() + ct));  
	    }  
	}  
  
	@Test  
	public void sideInput() {  
	    String[] word_arr = new String[]{  
	            "hi", "there", "hi", "hi", "sue", "bob",  
	            "hi", "sue", "", "", "ZOW", "bob", ""  
	    };  
	    List<String> words = Arrays.asList(word_arr);  
	  
	    PCollection<String> input = p.apply(Create.of(words));  
	  
	    // create view  
	    PCollectionView<Map<String, Long>> inputKeyCount = input  
	            .apply(Count.perElement())  
	            .apply(View.asMap());  
	  
	    // process with side input  
	    PCollection<KV<String, Long>> output = input  
	            .apply("after side input count", Count.<String>perElement())  
	            .apply("process side input", ParDo.of(new ProcessSideInputFn(inputKeyCount)).withSideInputs(inputKeyCount));  
	    PAssert.that(output).containsInAnyOrder(  
	            KV.of("hi", 8L),  
	            KV.of("there", 2L),  
	            KV.of("sue", 4L),  
	            KV.of("bob", 4L),  
	            KV.of("ZOW", 2L),  
	            KV.of("", 6L)  
	    );  
	    p.run();  
	}
}
```
