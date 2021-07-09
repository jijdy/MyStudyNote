#### ServletContext对象

* 概述：是一个全局对象，
* 当web服务器启动时，就会为每一个web应用程序创建一块共享的存储区域，即ServeltContext，在服务器启动时创建，在服务器关闭时销毁

* **获取ServletContext对象**：获取的对象都是同一个web应用中的同一个对象

~~~java
GenericServlet通过了getServletContext()方法，可通过this.getServletContext()方法获得
~~~

~~~java
HttpServletRequest通过了getServletContext()方法，直接获得ServeltContext对象
~~~

~~~java
HttpSession通过的getSeerveltContext()方法
~~~

* **获取项目信息**：

~~~java
/*
1. 通过serveltContext.getRealPath("/");获取当前项目在服务器发布的真实路径，绝对路径
2. 通过servletContext.getContextPath();可获得该项目所在的上下文路径，即该项目所属的web服务器路径

*/
~~~



* **用于存储全局信息**：因为为一个全局唯一变量，可以通过在其中存储数据进行数据的传输

~~~java
servletContext.setAttribute("name", value);//存储数据，以简直对的形式存储

serveltContext.getAttribute("name");//获取其中存储的键值对数据

servletContext.removeAttribute("name");//移除数据
~~~

* **应用于全局的数据记录**：例如记录访问人数的记录等