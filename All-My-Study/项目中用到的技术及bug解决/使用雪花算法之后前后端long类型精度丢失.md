### 前后端精度丢失

* 前端接收数据时使用number，其精度大概在17位左右，而后端的long类型的整数最大值可以达到19位，在进行前后端通信时，可能就会产生精度丢失的问题
* 在前后端进行数据传输时，一般都是使用JSON进行数据的序列化并进行传输
* 所以可以在后端将数据先转为String类型的数据，再拿到前端，就可以直接忽略其精度可能导致的问题



1. 直接在单个使用了雪花算法的字段上进行JSON数据的序列化操作(不推荐，单个操作太花时间)

~~~java
@JsonSerialize(using = ToStringSerializer.calss)
private Long id;
~~~

2. 在配置文件中进行统一配置，在文件在进行了JSON序列化的地方都先将Long类型转化为String类型向外传输，即使前端收到的数据为String类型的数据，相当于是为JSON的序列化操作做了一个过滤器操作。

~~~java
@Configuration
public calss JacksonConfig {
    @Bean
    public ObjectMapper jacksonObjectMapper(Jackson2ObjectMapperBuilder builder) {
        ObjectMapper objectMapper = builder.createXmlMapper(false).build();
        SimpleModule simpleModule = new SimpleModule();
        simpleModule.addSerializer(Long.class, ToStringSerializer.instance);
        objectMapper.registerModule(simpleModule);
        return objectMapper;r;
    }
}
~~~

