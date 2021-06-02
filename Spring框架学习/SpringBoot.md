#

* 使用maven构建springboot项目
	1. 创建一个唯一的main方法类，通过注解@SpringBootApplicaction注解标记该类并调用SpringApplication.run方法启动
	2. 通过直接创建controller控制层类添加注解使用
	3. 可以通过maven的packing功能将项目直接打包为可以运行的jar包