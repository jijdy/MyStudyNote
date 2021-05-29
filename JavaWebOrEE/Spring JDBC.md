# Spring JDBC
	* 是Spring框架对jdbc的简单封装，提供了一个JDBCTemplate对象简化了jdbc的操作
	
	* 步骤
		1. 导入jar包依赖
		2. 常见JDBCTemplate对象，依赖于DataSource数据库连接池
			* JDBCTemplate template = new JDBCTemplate(DataSource ds);
			
		3. 调用JDBCTemplate中的方法来完成crud的操作
			* updata():执行DML语句，增删改语句
			* queryForMap():查询结果将结果集封装为Map集合,将列名作为key，值为value，再将记录封装为一个map集合，只能查询一条记录
			* queryForList):查询结果将结果集封装为List集合,将每一天记录封装Wie一个map集合，再将map装载在list中 List<Map<Spring, Object>> list//
			* query():查询结果，将结果集封装为JavaBean对象
			* queryForObject():将结果封装为Object对象，常用于聚合函数的查询
			* 在调用方法是，同时若sql中有？通配符，则只需要按顺序写入数值即可
		4. 调用之后会自动将连接对象返回到连接池，也不需要去关闭额外的资源文件
		
	* 将查询到的记录封装为一个个对象并放入到集合中
	~~~
	//使用JDBCTemplate中的query方法返回一个JavaBeam对象并进行封装
	List<Class> list = jdbctemplate.query(sql,new BeanPropertyRowMapper<Class>(Class.class); 
	//使用NewBeanPropertyRowMapper<类型><类型.class)参数返回指定的对象
	~~~
		* 注意：在将值封装为对象时，8种基本数据类型是不能为null的，需要使用封装类型
		* 使用query()方法时打参数：RowMapper一般我们使用BeanPropertyRowMapper实现类，可以完成数据到javabean对象的自动封装