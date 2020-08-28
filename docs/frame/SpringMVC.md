## SpringMVC



> SpringMVC的主要功能是将Controller控制器放入mvc容器，实现Servlet的功能，并添加一些额外的功能，使web开发更容易。Controller并不是Servlet

1. SpringMVC运行原理

- web开发底层是Servlet，SpringMVC中有一个Servlet对象——**DispatcherServlet**
- DispatcherServlet（中央调度器）负责接收用户的请求
- 用户向服务器发送请求，DispatcherServlet接收请求后，将请求分配转发给对应的控制器





2. web配置

``` xml
<!--注册SpringMVC核心对象DispatcherServlet-->
<servlet>
	<servlet-name>springmvc</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <!--配置文件的位置，默认的读取规则：WEB-INF目录下Servlet名-servlet.xml-->
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:springmvc.xml</param-value>
</servlet>

<!--配置映射-->
<servlet-mapping>
	<servlet-name></servlet-name>
    <!--拓展名方式-->
	<url-pattern>*.do</url-pattern>
     <!--斜杠-->
	<url-pattern>/</url-pattern>
</servlet-mapping>
```

3. 接收参数

- RequestParam 依次接收多个参数
- 使用对象接收参数

​	

4. 返回值

- ModelAndView 数据和视图
- void
  - 处理ajax请求
- String 
  - 需要添加数据可以手动使用Request对象添加到Request作用域中
  - 表示逻辑地址，需要配置视图解析器
  - 表示完整路径，不能配置视图解析器
- Object

处理json格式的数据，实现步骤

- 引入处理json数据的工具库依赖，SpringMvc默认使用jackson
- 在配置文件中加入注解驱动（完成对java对象到json，xml，text，二进制等数据类型的转换，HttpMessageConverter接口，消息转换器）
  - HttpMessageConverter有很多实现类，实现了java对象到其他数据的转换
  - canWrite方法，检查java对象能不能转化成xx格式
  - write方法 ，调用jackson中的ObjectMapper将数据转为json数据

``` xml
<mvc:annotation-driven/>
```

- 在控制器的方法上加上@ResponseBody



浏览器的请求并不都是由框架处理的，tomcat处理静态资源以及未映射的Servlet请求



5. 静态资源请求的处理

   web.xml里有一个名为default的Servlet，项目中如果使用/ ，会替代这个Servlet

   ``` xml
   <!--配置映射-->
   <servlet-mapping>
   	<servlet-name>myweb</servlet-name>
   	<url-pattern>/</url-pattern>
   </servlet-mapping>
   ```

   

