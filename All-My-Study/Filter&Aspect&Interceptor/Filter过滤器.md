#### Filter

* 通过实现Filter接口(javax.servlet.filter)来实现过滤器
* 需要实现init(); doFilter(); destory();三个方法

~~~java
public class Myfilter implements Filter {
    @Override
    public void inti(FilterConfig filterConfig) throws ServletException {
        //初始化函数，一般只重写，不进行；具体的实现
    }
    
    @Override
    public void doFilter(ServletRequest servletRequest, ServeltResponse servletSreponse, FilterChain filterChain) {
        //具体需要进行的过滤器逻辑
        //将请求和响应通过过滤链向下传递,即进入到业务逻辑中，在执行完毕进行返回时，返回数据仍然会进入到过滤器中，再通过过滤器传输到前端客户端
        filterChain.doFilter(servletRequet, servletResponse);
        
    }
    
    @Override
    public void destory(){
        //可以重写，也可以不重写，但一般也并不进行实现
    }
}
~~~

* **过滤器的配置设置**：在Servelt应用中，注解通过@WebFilter(value="/xxx")； xml通过添加filter和filter-mapping标签进行设置

#### 过滤器链和优先级

* 在有多个过滤器时，所在在调用filterChain.doFilter()；方法之后，会先检查之后是否还有Filter需要进行工作，有则进入下一个Filter过滤器，直到无，进入业务处理，调用执行组成一条链路结构。反向响应也是一条条的向上响应
* 优先级按照xml中配置的顺序，注解中的url的先后顺序

#### 过滤器的典型应用

* 使用过滤器进行请求和响应的编码格式的转换

~~~java
public void doFilter(ServeltRequest servletRequest, ServeltResponse servletResponse, FilterChain filterChain) throws IOException, ServletExceprion {
    //统一转换请求的编码格式，以防止乱码的出现
    servletRequest.setCharacterEncoding("UTF-8");
    serveltResponse.setContentType("text/html;charset=utf-8");
    
    filterChain.doFilter(servletRequeest, servletResponse);//过滤链传递
}
~~~

* 使用过滤器进行权限验证；Filter中传入的参数都是Servlet下的请求返回类，通过向下转型的方式将Filter中的请求响应对象强转为HttpServlet下的请求响应对象。；调用req.getSession();可获取到其Session中的数据信息，之后可进行业务逻辑的判断

