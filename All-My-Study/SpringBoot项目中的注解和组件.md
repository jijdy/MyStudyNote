### 用到的注解

~~~java
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;
~~~

* @ControllerAdvice： 用于控制层参数校验
* @ExceptionHandler：用于拦截出现的异常并进行相应的操作，例如打印日志
* @ResponseBody：将要返回的java对象转换为JSON对象，并直接返回给HTTP前端界面的拦截器，如Ajax或axios。



* @Resource/@Autowired：都用来实现依赖注入，@Autowired属于Spring，并且默认按照类型(byType)进行上下文的查找和bean类型的自动匹配，而@Resource属于J2EE，默认按照byName来进行装配。



* @Configuration：声明该类为个配置类，用于自动装配Spring容器，一般用于类上，这个类中就可以有一个或多个bean类型的注解，其基本等价于简化了Spring中的bean配置，不需要在xml中些配置。通俗的讲 @Configuration 一般与 @Bean 注解配合使用，用 @Configuration 注解类等价与 XML 中配置 beans，用 @Bean 注解方法等价于 XML 中配置 bean。 -==== 也可以通过@Resource来对其进行替代使用，不使用@bean注解而使用@Resource注解来进行依赖注入

#### 启动类注解

~~~java
@ComponentScan("com.example.mywiki")
@SpringBootApplication
@MapperScan("com.example.mywiki.mapper")
~~~

* @ComponentScan("包名")：自动扫描组件，用于扫描该包下带有Commponent注解  (包括M,V,C和其他注解的注解)  的类并将其添加到Spring容器中，其可以使用注解中的excludeFilters 和 includeFilters 属性来对其进行扫描过滤。
* @MapperScan("包名"，"包名")：添加包名，包中的所有接口都会在Spring初始化容器之后生成对应的实现类，以生成映射关系



### 核心注解及SpringBoot的启动机制

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
```

* @SpringBootApplication：是SpringBoot中的最核心注解，用于开启自动配置和启动SpirngBoot项目的前置条件。
  * @ComponentScan：用于扫描被@Component注解标注的类，用于交给SpringBoot管理并注入bean容器。也即是控制反转IOC的实现方式。不过虽然其中含有这个扫描注解，但也可以在启动类上添加@ComponentScan注解进一步标点其扫描的范围，而不需要关系SpringBoot的启动类的位置。
  * @EnableAutoConfiguration：**借助@Import的支持，收集和注册特定场景相关的bean定义**。其借助@Import(AutoConfigurationImportSelector.class)注解使用AutoConfigurationImportSelector将所有被@Configuration注解标注的类加载并注入到ioc容器中。
  * SpringFactoriesLoader类：其主要是从特点的配置文件META-INF/spring.factories中加载配置。配合@EnableAutoConfiguration注解使用时，@EnableAutoConfiguration注解会优先找到这个加载文件并将其中的配置汇总为一个配置类并加载到ioc容器中。
  * @SpringBootConfiguration：其功能和@Configuration注解功能类似，都是声明这个类为配置类并加载到ioc容器中
  * @Inherited ：表示这个使用这个注解的类被继承了之后。我子类也会继承这个注解。表示注解继承的概念。
  * @Documented： 用于记录的注解
  * @Retention()：其中RetentionPolicy是一个枚举类，拥有标定这个注释的保留时间
  * @Target()：表示该注解的可标定范围，类或方法或属性等等。
  
* **SpringBoot的启动流程机制**：有SpringApplication类的run()方法来实现项目的启动

~~~java
public static void main(String[] args) {
    SpringApplication app = new SpringApplication(MyApplication.class);
    Environment nev = app.run(args).getEnvironment();
    //SpringApplication.run(MyApplication.class, args); //寻常实现方式
}

~~~

1. 首先向SpringApplication的实例对象中传入启动类的class对象，根据这个传入的实例对象的classpath
* 使用SpringFactoriesLoader在应用的classpath中查找并加载所有可用的ApplicationContextInitializer。（应用程序上下文初始化器）
* 使用SpringFactoriesLoader在应用的classpath中查找并加载所有可用的ApplicationListener。（应用程序监听器）
2. 首先使用在上下文中拿到的所有应用监听器，分别遍历的调用它们的SpringApplicationRunListener类的started()方法，使其启动
3. 创建并配置SpringBoot需要用到的Environment配置，包括主机编号和端口号等
4. 环境准备好了之后，再次调用监听器SpringApplicationRunListener的environmentPrepared()方法，加载环境变量并进行下一步动作
5. 若使用了banner，则打印横幅，启动的图像，代表SpringBoot开始启动
6. 通过前置配置创建ApplicationContext并将环境参数传入其中
7. 创建好应用程序上下文之后，再次使用SpringFactoriesLoader遍历查找加载classpath下的ApplicationContextInitializer类并调用initialize(applicationContext)方法完成上下文的初始化操作
8. 在完成初始化操作之后，再将之前通过注解等方式获取到的ioc容器中的配置类加载到准备完毕的ApplicationContext中
9. 再遍历调用SpringApplicationRunListener监听器的contextLoaded()方法，告知监听器初始化操作已经完成
10. 完成上下文的基本配置后，再调用ApplicationContext的refresh()方法，完成IOC容器的所有与初始化操作
11. 查找ApplicationContext中的所有存在的CommandLineRunner，遍历的执行这些命令行操作
12. 最后，再调用SpringApplicationRunListener的finished()方法，完成整个时间的监听过程。



##### mvc等组件

* @component：实现了bean的注入，将这个类交给Spring容器来进行管理

* @Controller 控制器（注入服务）， 用于标注控制层，并用于向前端或客服端开放HTTP连接需要的资源。
* @Service 服务（注入dao），用于标注服务层，主要用来进行业务的逻辑处理
* @Repository（实现dao访问），用于标注数据访问层，也可以说用于标注数据访问组件，即DAO组件
*  @Component （把普通pojo实例化到spring容器中，相当于配置文件xml）泛指各种组件，就是说当我们的类不属于各种归类的时候（不属于@Controller、@Services等的时候），我们就可以使用@Component来标注这个类，以便Spring能够扫描到其中的有关注解和信息。

##### 定时任务注解

* @EnableScheduling：在SpringBoot的启动类上添加，表示支持自动定时功能并对全局ioc容器中的定时方法进行扫描，完成定时任务
* @Scheduled：在方法上进行声明(其所在类要被@Componen修饰，注入容器)，表示要进行定时任务的执行。
  * corn：使用corn表达式进行定时，如" 0 0/5 * * * ?"表示没5分钟执行一次，若下一次执行时上一次的执行未完成，则推迟执行
  * zone：表示执行时间的时区
  * fixedDelay和fixxedDelayString：表示一个固定的言辞执行时间，到这次任务执行结束后，下一次执行的间隔 @Scheduled(fixedDelayString= "2000")表示2s的时差
  * fixedRate 和fixedRateString：基本同上，但是间隔为这次开始之间和下次开始市价，如错过，则跳过
  * .initialDelay 和initialDelayString：表示一个初始延迟时间，第一次被调用前辈延迟的时间，只用一次调用的时间

##### 异步化注解

* @EnableAsycn：在SpringBoot的启动类上使用，来使SpringBoot支持异步操作
* @Asycn：使用在需要进行异步的类或方法上添加，添加之后在进入到这个类或方法时会重新开启一个线程进行异步执行，调用这个异步化的类或方法的对象会继续执行之后的操作。
  * 其和websocket的启动注解有冲突，需要重新封装一个类来使用

##### 事务

* @Transactional：事务注解，由于添加在类或方法上进行一致性的操作
* 事务注解运用在方法上时，若其中同一个类上对事务方法进行调用，则事务部起作用

##### 参数校检相关

~~~java
@JsonFormat(pattern="MM-dd", timezone = "GMT+8")
@DateTimeFormat(pattern = "yyyy-MM-dd")
~~~

* @JsonFormat：timezone=" "表示时区的设置，一般国内都设置GMT+8，表示上海时间；pattern表示设置输出时间的具体格式"yyyy-MM-dd"表示年月日；一般用于和数据库交互的参数上进行设置，用于将从数据库中取出的日期格式信息进行归整化
* @DateTimeFormat：也是设置时间格式的，但是此表示将前台的日期格式化为数据库需要存储的日期格式
* @JsonFormat主要用于从后台数据库中取出日期进行格式化；@DateTimeFormat用于前台向后台的时间格式化；

### 用到的组件

* DigestUtils：使用该组件来对字符串进行md5值的加密存储和显示

~~~java
import org.springframework.util.DigestUtils;

req.setPassword(DigestUtils.md5DigestAsHex(req.getPassword().getBytes()));
~~~

* RedisTemplate：是一个jedis的高度封装工具类，直接对redis进行简单的操作，



* logback组件，在SpringBoot中就有集成，用于日志打印，日志级别fatal ===>> error ===>> warn ===>> info ===>> debug ===>> trace 一般使用最多的就是info，warn，error三种级别的日志，当有error级别的日志出现时，服务器就有可能会出现问题

