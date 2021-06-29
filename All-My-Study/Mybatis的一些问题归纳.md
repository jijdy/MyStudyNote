### Mybatis的一些问题

#### 1. Mybaits中如何解决sql注入问题

* 使用#{}和${}的区别

~~~xml
  <delete id="deleteByPrimaryKey" parameterType="java.lang.Long">
    delete from demo
    where id = #{id,jdbcType=BIGINT}
  </delete>
~~~

* 在Mapper.xml文件中的sql语句中，使用#{}来接收传入参数，可以将这个位置的参数作为一个占位符，在预编译时将这个参数与自动转化为一个字符串，即将上方sql预编译为

~~~sql
DELETE FROM `DEMO` WHERE `id` = `id`;
~~~

* 这样的处理方式就等同于sql中使用？来有效防止sql注入
* 若使用${}来接收传入的参数，则一些恶意的sql注入就会发生，
* 如果关注一些mybatis自动生成的mapper层代码相关的xml文件，就会发现其自动生成的代码几乎全都使用的#{}来进行参数的接收，所以在自己进行自定义mapper方法时，若无必要，尽量都使用#{}为佳

#### 2. Mybatis的一，二级缓存

1. mybatis的一级缓存为默认开启，其主要是sqlSession级别的缓存，在一个方法对sql进行查询之后，就会开启一个sqlSession，在这个相同的sqlSession进行下一次增删改之前，这个缓存都是有效且不变的。一般是用于提高数据查询的速度，并且一般不会发生数据脏读等问题。
   * sqlSession对数据库的操作全都依赖于**Executor**接口及其实现类(主要是**BaseExecutor**抽象类及其子类)，来对数据库进行相关操作
   * 而sqlSession的一级缓存实现依靠**PerpetualCache**中所持有的HashMap进行存储，所以科研所对一级缓存的操作基本就是对HashMap的操作
   * sqlSession的一级缓存在其进行commit()方法和close()等有关对数据库的数据进行修改时，就会刷新该缓存的数据
   * mybatis和spring整和进行mapper代理开发，不支持一级缓存，一般会默认关闭
2. 二级缓存是Mapper级别的缓存，一般不是开启的，因为很可能会发生脏读的问题。其是主要针对一个表级别的缓存，在这个表对应mapper中的多个sqlSession可以共同对一个缓存空间进行数据的查询，具有很高的查询速度(因为不需要经常到数据库中拿取数据)，但是会有很高的风险
   * 一般不推荐开启

