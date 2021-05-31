# JDBC的基本使用：

### 基本概念

* java database connection 用于java和数据库的链接和使用
* jdbc本质上是一套接口，又官方定义，各个数据库厂商实现接口，并提供不同的数据库驱动jar包

### 使用步骤：
	1. 将数据库的驱动jar包导入项目中
	2. 注册驱动
	3. 获取数据库连接对象Connection
	4. 定义sql
	5. 获取执行sql语句的对象 Statement
	6. 执行sql，接收返回结果
	7. 释放资源
	
	~~~
	//1.导入jdbc包到lib资源路径下
	//2.注册驱动，将这个类加载入内存
	Class.forName("com.mysql.jdbc.Driver");
	//3. 获取数据库连接对象Connection
	Connection con = DriverManager.getConnection("jdbc:mysql://localhost:3306/databaseName","userNmae","password");
	//4. 定义sql语句
	String sql = “要执行的sql语句”；
	//5. 获取执行sql语句的对象 Statement
	Statement stat = con.cteateStatement();
	//6. 执行sql，接收返回结果
	int flag = stat.excute--(sql); //execute执行，其中不同语句有不同的execute方法executeUpdate,executeQuery,常用
	//7. 关闭资源，相当于io的资源关闭
	stat.close();
	con.close();
	~~~

### 常见的对象即类
	1. DriverManager：驱动管理对象
		* 功能
			1. 注册驱动
				在将Driver类加载时其中的一个静态代码块会调用其的registerDriver(new Driver())方法进行驱动的注册  
				mysql5之后的jar包有自动注册的功能
			2. 获取数据库连接
				* static Connection getConnection(String url, String user, String password);  
				* url:指定连接的路径：
					* jdbc：mysql://ip地址(域名):端口号/数据库名称
	2. Connection: 数据库连接对象  
		1. 获取执行sql语句的对象：
			* Statement createStatement(String sql);
			* PreparedStatement PreparedStatement(String sql);
		2. 管理事务
			* 开启事务：setAutoCommit(boolean autoCommit); //关闭自动提交，则Wie开启事务
			* 提交事务：commit();
			* 回滚事务: rollback();
	3. Statement：执行sql的对象
		1. 执行sql语句
			* execute() //可执行所有sql语句，但不常用
			* int executeUpdate(String sql) //执行DML和DDL语句，返回值为影响行数 >0 则执行成功
			* ResultSet executeQuery(String sql);//执行DQL(select)语句
	4. ResultSet: 结果集对象，封装查询语句的返回值
		* Boolean next();将光标向下移动一行，若为false则为最后一行，无数据
		* getXxx(参数);
			* Xxx: 可以是数据类型Int，String等，
			* 参数：
				1. int：代表列的编号，从1开始,如getStrign(1)
				2. String: 代表列名称。如getInt("列名称");
		* 注意：
			* 游标开始与表中的列名
			* 游标向下移动一行，判断是否有数据，while
			* 再对结果集进行操作
	5. PreparedStatement:  Statement的子类(子接口)，也是执行sql语句的对象，但功能更加强大
		1. 用于解决sql注入问题的预编译sql语句
		2. 参数中使用?作为占位符
			* 例：sql语句中：select * from user where username = ? and password = ? ;
		3. 通过 PreparedStatement Connection。preparedStatement(String sql); //在第一对象的时候就将sql语句参数传入其中
		4. 给？赋值
			* setXxx(参数1，参数2);
			* 参数1为？的位置，1开始 ；参数2位？的值
		5. 通过PS对象调用execute执行方法，不需要传递参数
		6. 之后大多使用此对象来完成crud的操作
			* 可以防止sql注入问题
			* 效率相对其父类更高
		
#### sql注入问题
	* 在拼接sql语句时，有一些sql的关键字参与了字符串的拼接，所造成的安全问题
	* 登录中：只要密码中输入：a' or 'a' = 'a'
	*        则会将之后的or判断直接在数据库中生效，无论密码正确与否，都会返回一个true值

#### 从properties配置文件中读取信息
	* 一般需要读取配置文件想在类加载时就完成，所以一般可以将读取操作仿真static静态代码块中完成，在类被初始化的时候就完成了配置文件的读取
	1. 创建properties集合类
		Properties Pro = new Properties();
	2. 加载文件
		pro.load(new FileReader("文件所在路径"))；
	3. 获得文件中的值
		url = pro.getProperty("url");
	4. 抛出异常等等
	
	* 文件路径的动态获取 ClassLoader类加载器获取
		1. ClassLoader classLoader = ClassName.class.getClassLoader(); //获取类加载器
		2. URL url = classLoader.getResource("配置文件名"); //URL表示一个文件路径或地址的类
		3. String path = url.getPath(); //获取路径名称的字符串
		4. 之后就可以使用这个path来对文件进行加载了
	
### JDBC控制事务
	* 使用Connection的的事务管理方法
	* 在执行sql前开启事务setAutoCommit(false);
	* 在sql全都执行完成之后进行提交commit();
	* 如果在执行过程中出现了异常进入到异常处理语句中则回滚事务rollback();