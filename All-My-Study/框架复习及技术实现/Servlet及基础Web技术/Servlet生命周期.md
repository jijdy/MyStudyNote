#### Servlet的生命周期

1. **实例化** ：在用户第一次调用这个Servlet方法时，由容器调用Servlet的构造器函数来创建一个具体的Servlet对象。当然也可以在容器以创建的时候就实例化Servlet对象。一般一个Servlet类只会创建一次
2. **初始化** ：调用Servlet接口中的init方法，其传入的参数为ServletConfig类型的对象
3. **服务** ：进行Servlet的使用，若前端服务器有请求来到了Servlet，则就会调用其中的方法，从前端传入Request到后端进行处理并响应Response给前端，service方法，一般也可以当做doGet等方法
4. **销毁**：当Servlet容器停止或则重新启动时，都会触发Servelt的destory()方法将Servlet进行销毁，销毁之后可以再次通过容器进行实例化和初始化

