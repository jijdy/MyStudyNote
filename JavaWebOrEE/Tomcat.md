# Tomcat服务器
	1. 官网下载压缩包，解压
	2. 需要提前配置好jdk环境，Tomcat由java语言编写，兼容性配置，编码格式配置
	3. 在conf包下的server.xml配置核心属性
		* 启动端口<Connector port=8080
		* http端口默认为80，https端口默认为443
		* 可以配置主机的的名称(localhost/127.0.0.1) ，也可以通过(system32/drivers/etc/hosts配置文件)修改主机的127.0.0.1的名称修改其连接的地址
		* 默认存放网站运用的位置：webapps
	4.发布一个web网站
		1. 将自己些的网站，放在服务器(tomcat)中指定的web应用的文件(webapps)就可以访问了
		2. 网站中的结构
		~~~
		--webapps：Tomcat服务器的web目录
			--ROOT(web包)
				--WEB-INF
					-classes:java程序字节码文件
					-lib:web应用所依赖的jar包
					-web.xml：网站配置文件
			--index.html/index.jsp:网站首页
			--static
				-css
				-js
				-img
			--... 
		~~~
	5. 项目部署
		* 一般使用war包直接放到webapps目录下，会自动解压，若删除war包，也会自动删除已解压的文件
		* 配置conf/server.xml文件
			-- 在<Host> 标签体中配置
			 -- <Context docBase="文件路径" path="/xx" />  //path为网站访问的虚拟地址
		* 在conf/Catalina/localhost创建一个任意的xml文件在文件中编写<Context >
			* path="/xx"表示工程的访问路径，docBase表示工程目录的位置
			-- 不需要配置path，虚拟地址为docBase目录的文件名
	* 若访问的地址无其余url，则默认为webapps中的ROOT资源工程
	6. 在idea中创建一个动态的web工程：在java Enterprise中创建项目
	