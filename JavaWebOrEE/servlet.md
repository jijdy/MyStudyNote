# Servlet
	* 概念 server applet 运行在服务器(tomcat)上的小程序
		* servlet就是一个接口，定义了java类能够被浏览器访问到tomcat的规则
		* 自定义一个类，实现servlet接口，复写方法
		* 是javaWeb的三大组件之一：Servlet程序，Filter过滤器，Listener监听器
		* 作用：可以接受来自客户端发送过来的请求，并响应数据给客户端
	* 具体使用：(实现一个Servelt接口使用)/一般通过继承HttpServletRequest类来使用已经封装好的半成品servlet
		1. 创建一个javaEE项目
		2. 定义一个类，实现Servlet接口
			* public class ServletDemo implement Servlet(){}
		3. 实现Servlet接口中的抽象方法
		4. 在web前端配置文件(xml文件)中配置Servlet
			<servlet>
				<servlet-name><>
				<servlet-class>(在项目中的类路径)server.servlet.Servlet<>
			</servlet>
			<servlet-mapping>
				<servlet-name><>（同上）
				<url-pattern>/demo1<> //表示在网页中的url定位名称
			<servlet-mapping>
			* 两个标签相互映射，一个是哪出类class，一个是在前端网页中设定url地址
	* Servlet的生命周期
		1. 执行Servlet构造器方法
		2. 执行init初始化方法(在第一次访问servlet程序时调用)
		3. 执行service方法(每次访问都会调用这个方法)
		4. 执行destroy销毁方法(在web工程停止时调用)
	
	* 实现servlet接口，
		1. 必须实现service方法，同时通过HttpServletRequest对象将强转为此对象的请求信息做处理
		2. 一般使用HttpServletRequest类中doGet()，doPost()方法进行重写
		3. 返回信息
	* 一般在实际项目中都是通过基础HttpServlet类来实现Servlet重写
		1. 构建时若不使用呢注解@WebServlet来进行配置，就需要自己在xml文件中配置属性
		2. 需要重写其中的doGet和doPost方法来实现接收页面的；请求
	
	* Servlet类中的继承关系
		1. 初始接口Servlet
		2. GenericServlet实现了Servlet接口的初始功能，做了很多空实现(大部分)，并持有ServletConfig类的引用，对其的使用做了一些方法
		3. HttpServlet继承了GenericServlet类以便操作，实现了service方法，只需要实现doGet和doPost方法即可直接使用
			* 首先将请求和响应类型进行强转，再判断其是get还是post(String method = req.getMethod()),只负责检测异常和抛出，是否支持get/post
			请求
		4. 自己实现类来继承HttpServlet类，根据需要再重写两个方法
	
	* ServletConfig类
		1. 可以获取servlet程序的别名servlet-name的值 getServletName();
		2. 可以获取servlet程序中的init初始化参数 getInitParameter(String name)；
		// 在web.xml文件中的一个servlet标签中的<init-param>中标签定义的键值对
		3. 获取servlet的context对象 ServletContext getServletContext();
		4. Enumeration<String> getInitParameterNames();
		
	* servlet程序和servletConfig对象都是由Tomcat负责创建，servlet程序默认为第一次访问时创建，而一个config对象对应一个servlet对象，
	
	* ServletContext接口
		1. 是一个接口，表示servlet上下文对象
		2. 一个web过程，只有一个ServletContext对象实例
		3. ServletContext对象是一个域对象
		
	* 域对象：
		1. 存数据：Map对象put();域对象setAttribute(String name, Object object)
		2. 取数据：get();  Object getAttribute(String name)
		3. 删除数据：remove();  removeAttribute(String name)
	* ServletContext类的四个作用：
		1. 获取web.xml中配置的上下文参数<context-param>
		2. 获取当前工作路径 格式：/工程路径
		3. 获取工程部署后在服务器硬盘上的绝对路径
		4. 像map一样获取数据，以键值对的方式
	
	