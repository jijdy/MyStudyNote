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

* @Controller 控制器（注入服务， 用于标注控制层。
* @Service 服务（注入dao），用于标注服务层，主要用来进行业务的逻辑处理
* @Repository（实现dao访问），用于标注数据访问层，也可以说用于标注数据访问组件，即DAO组件
*  @Component （把普通pojo实例化到spring容器中，相当于配置文件xml）泛指各种组件，就是说当我们的类不属于各种归类的时候（不属于@Controller、@Services等的时候），我们就可以使用@Component来标注这个类，以便Spring能够扫描到其中的有关注解和信息。



### 用到的组件

* DigestUtils：使用该组件来对字符串进行md5值的加密存储和显示

~~~java
import org.springframework.util.DigestUtils;

req.setPassword(DigestUtils.md5DigestAsHex(req.getPassword().getBytes()));
~~~

