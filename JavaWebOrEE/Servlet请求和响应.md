# 

* http请求的协议内容：
	* (GET请求的http报文结构)
	1. 请求行 ：
		1. 请求方式：GET/POST
		2. 请求的资源路径(url资源定位符或加？value=xx)：/xxx/xx/xxx/xx/x？value=
		3. 请求的协议版本号：http一般都是 HTTP/1.1
	2. 请求头：以键值对方式的信息
		1. Accept：告诉服务器客户端可以接收的数据类型(文件类型jpg等)
		2. Accept-Language：告诉服务器客户端可以接收的语言类型
			* zh_CN   中文中国
			* en_US   英文美国
		3. User-Agent：用户代理，即浏览器的信息
		4. Accept-Encoding:告诉服务器客户端可以接收的数据编码(压缩的)格式，utf-8
		5. Host：表示请求的服务器ip和端口号
		6. Connection：告诉服务器请求连接如何处理
			* Keep-Alive：告诉服务器回传数据之后不要马上关闭，保持一小段时间的连接
			* Closed：发送完请求之后就关闭
	* (POST请求的http报文结构)
		1. 请求行：同上
		2. 请求头：key：value   以键值对的方式发送的信息
			* Referer：表示请求发起时，浏览器地址栏中的地址(记录信息的来源)，url信息
			* Contet-Type：表示发送的数据的类型
				* application、x-www-form-urlencoded :表示提交的数据格式为：name=value&name=value...
				* multipart/form-data: 表示以多段的形式提交数据给服务器(以流的形式，用于数据上传)
			* Context-Length: 表示发送的数据的长度(字母长度)
			* Cache-Control：表示如何控制缓存，no-cache不缓存
		-----空行-----
		3. 请求体: 发送给服务器的数据

* 常见的GET/POST请求
	* GET
		1. form标签中的method=get
		2. a标签
		3. link标签引入css
		4. script标签引入js文件
		5. iframe引入html页面
		6. img标签引入图片
		7. 在浏览器地址栏中输入地址
	* POST
		1. form标签中method=post
	
* http协议的响应
	1. 响应行
		1. 响应的协议和版本号
		2. 响应状态码(200)
		3. 协议状态描述符(ok)
	2. 响应头
		* 以key-value的形式发送
		* Server：表示服务器的信息
		* Content-Type:表示响应体的数据结构(text/html)
		* Context-Length:响应体的长度
		* Date：请求响应的数据(格林时间)
	----空行-----
	3. 响应体 ：回传给客户端的数据,一般为标签语言
* 常见的响应状态码：
	1. 200表示请求成功
	2. 302表示请求重定向(表示这个地址已经更新，会返回这个新地址给客服端客服端会再次向服务器访问这个新地址)
	3. 404表示请求服务器已经收到了，但是你要的数据不存在(请求地址错误
	4. 502表示请求服务器已经收到了，但是服务器发送了内部错误(代码错误
	