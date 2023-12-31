[[Beam]]
## Examples
**1 任意IO + Json转换 + 写入Clickhouse**

一个读取字符串输入流并解析json 为java bean 然后写入到 clickhouse 的pipeline
```java
public class SimPipeline {  
    public static void main(String[] args) {  
        ObjectMapper om = new ObjectMapper();  
        // init pipeline  
        PipelineOptions options = PipelineOptionsFactory.fromArgs(args).withValidation().create();  
        options.setRunner(DirectRunner.class);  
        Pipeline pipeline = Pipeline.create(options);  
        // read data, there use TextIO, more io connector see: https://beam.apache.org/documentation/io/connectors/  
        pipeline.apply("read from text file", TextIO.read().from("/var/sim_test.txt")) // there can replace with other connector IO.  
                .apply("convert json to java bean test", ParDo.of(new DoFn<String, Sim>() {  
                    @ProcessElement  
                    public void process(ProcessContext c) throws JsonProcessingException {  
                        String json = c.element();  
                        System.out.println("-------input in [convert json to java bean test]: " + json + "--------");  
                        Sim sim = om.readValue(json, Sim.class);  
                        c.output(sim);  
                    }                }))                .apply("element peek", MapElements.via(new SimpleFunction<Sim, Sim>() {  
                    @Override  
                    public Sim apply(Sim input) {  
                        System.out.println("-------input in [element peek]: " + input.toString() + "--------");  
                        return input;  
                    }                }))                .apply(ClickHouseIO.write("jdbc:clickhouse://10.226.136.231:8123/default", "sim"));  
        // run pipeline  
        pipeline.run().waitUntilFinish();  
    }
}

//@DefaultCoder(AvroCoder.class)  
@DefaultSchema(JavaBeanSchema.class)  
public class Sim {  
    private String name;  
    private int level;  
  
    public String getName() {  
        return name;  
    }  
    public void setName(String name) {  
        this.name = name;  
    }  
    public int getLevel() {  
        return level;  
    }  
    public void setLevel(int level) {  
        this.level = level;  
    }  
    public Sim() {  
    }  
//    @SchemaCreate  
//    public Sim(String name, int level) {  
//        this.name = name;  
//        this.level = level;  
//    }  
    @Override  
    public String toString() {  
        return "SimplePojo{" +  
                "name='" + name + '\'' +  
                ", level=" + level +  
                '}';  
    }
}
```