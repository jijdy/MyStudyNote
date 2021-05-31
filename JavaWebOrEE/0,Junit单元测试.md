### Junit单元测试工具  
	1. 导入依赖：
	  import java.junit.* 
	
	2. 在类前加入@test注解使其能够成为类似main的方法
	
	3. 使用@Before和@After注解在test前或后对测试单元进行进行统一操作
		* 例如在test之前进行init操作，或test之后进程close操作等