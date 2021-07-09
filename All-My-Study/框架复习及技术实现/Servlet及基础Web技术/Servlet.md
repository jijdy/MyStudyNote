### Servlet简易学习及了解

#### 实现Servlet接口

* 需要重写

~~~java
init(ServletConfig config);//初始化
ServeltConfig getServletConfig();//获取配置属性
service(ServletRequest req, ServeltResponse res);//主方法，用于编写业务服务
String getServletInfo();
destory();//销毁Servelt的方法
~~~

#### 一般的实现方法

* 继承GenericServlet抽象类

~~~
是一个抽闲类，但是只需要实现service方法即可简易的重写一个Servlet类
~~~

* 重写HttpServlet类

~~~
其在继承GenericServlet的基础上进一步扩展，
一其中实现了为web支持的Htpp相关协议的方法
doGet,用于servlet支持HTTP GET请求
doPost,用于支持POST请求
doPut,
doDelete,

其中对http中发送过来的方法进行判断，若为get方法，则调用doGet方法，依照此来进行业务逻辑的实现
~~~

#### HttpServletRequest请求类及常用方法

* 一般使用HttpServletRequest类来进行数据的请求

* 若请求信息在请求头URL中，则使用getParameter("username");来获取到请求头中传入的数据，一般在GET方法中使用较多，即在获得URL中的数据一般为xxx.com?username="xxx" 这样的格式，即可获取到传入的参数的参数名

~~~java
protected void doGet(HttpSeervletRequest req, HttpServeltReponse resp) throws ServletException {
    String username = req.getParmeter("username");
}
~~~

* `StringBuffer request.getMethod();`表示获取此次传输数据所用的方法，GET,POST
* `String request.getHeader("name");`可通过请求头header中的信息匹配出其中的相关信息
* `request.getRequestURL().toString();`可以获取到http请求的URL
* `request.getRemoteAddr();`获得远程访问的ip地址信息，若使用了Nginx进行反向代理，则不能够直接得到用户访问地址。
* 

#### HttpServletResponse响应类及方法

* `setHeader(name,value); `设置响应头中的header信息，以键值对的形式存放
* `setContentType(String ); `设置响应文件类型，响应式的编码格式"UTF-8"；例："text/htm;charset=utf-8"
* `setCharacterEncoding(String ); `设置服务端响应内容的编码格式
* getWriter(); 获取字符输出流，并在resp中写入数据，直接将数据传输到前端

* `response.setStatus(HttpStatus.UNAUTHORIZED.value());`设置返回响应的状态码，一般不需要设置，非常规时设置该信息，其中传入的是HttpStatus中的枚举对象的值
