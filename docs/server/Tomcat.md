## Tomcat



> Tomcat 服务器是一个免费的开放源代码的Web 应用服务器，属于轻量级应用[服务器](https://baike.baidu.com/item/服务器)，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP 程序的首选。



#### 1. 配置文件

配置端口号

```xml
<Connector port:"8080" protocol="HTTP/1.1"
           connectionTimeout="20000"
           redirectPort="8443"/>
```



配置主机名称

- 默认主机名：localhost->127.0.0.1
- 默认网站应用存放位置：webapps

```xml
<Host name="域名" appBase="webapps"
      unpackWARs="true" autoDeploy="true">
```



#### 2 .网站是如何进行访问的

1. 输入域名，发送请求
2. 检查本机host配置文件有没有这个域名的映射

- 有：直接返回对应的ip地址
- 没有：去DNS（域名系统）服务器



#### 3. Servlet运行原理

1. 什么是Servlet？

Servlet是j运行在服务器上的java程序，他是为动态web而生，

实现Servlet有三种方式：

- 实现Servlet接口
- 继承GenericServlet类
- 继承Httpservlet类（最常用）



2. Servlet生命周期

- 加载和实例化

Servlet容器负责Servlet的加载和实例化，容器通过类加载器加载Servlet类。容器通过java反射创建Servlet实例，调用的是Servlet的默认构造参数，所以实现Servlet类的时候，不应该提供带参构造器

- 初始化 

容器调用Servlet的init()方法进行初始化，在初始化过程中，可以通过ServletConfig对象或web.xml获取配置信息

- 请求处理

调用service()方法

- 销毁

调用destroy()方法



3. Servlet执行过程

1. 浏览器向服务器发送请求
2. Tomcat接收请求，产生请求对象HttpServletRequest和响应对象HttpServletResponse
3. Tomcat根据请求url找到指定的Servlet
4. 调用service()方法处理请求
5. 销毁线程





tips：

- Tomcat启动时进行Servlet容器加载和实例化Servlet
- 可以通过配置设置Servlet初始化的时间

```xml
<load-on-starup></load-on-starup>
```

标记容器是否在启动的时候就加载这个servlet，如果未配置此项，**Servlet一般在第一次响应web请求时检测Servlet是否初始化， 如未初始化，则进行初始化后再响应请求**。

当值为0或者大于0时，表示容器在应用启动时就加载这个servlet；

当是一个负数时或者没有指定时，则指示容器在该servlet被选择时才加载。

正数的值越小，启动该servlet的优先级越高。（指定Servlet加载顺序）

- Servlet为每个请求分配一个新线程





#### 4.Servlet配置



1. 配置文件

```xml
<servlet>
         <!-- servlet的内部名称，自定义 -->
        <servlet-name>DemoAction</servlet-name>
        <!-- servlet的类全名：包名+类名 -->
        <servlet-class>com.uplooking.controller.DemoAction</servlet-class>
        <load-on-startup>1</load-on-startup>
</servlet>
<!-- servlet的映射配置 -->
<servlet-mapping>
        <!-- servlet的内部名称，一定要和上面的内部名称保持一致 -->
        <servlet-name>DemoAction</servlet-name>
        <!-- servlet的映射路径（访问serclet的名称 -->
        <url-pattern>/DemoAction</url-pattern>
</servlet-mapping>
<!-- 初始化参数 -->
<init-param>
       <param-name>abc</param-name>
       <param-value>123</param-value>
</init-param>

<!-- 上下文参数 -->
<context-param>
       <param-name>abc</param-name>
       <param-value>123</param-value>
</context-param>
```



Servletconfig对象

- getInitParameter(String param) 读取初始化参数
- getInitParameterNames() 读取所有初始化参数名（枚举类型）

ServletContext对象

- getInitParameter(String param) 获取上下文参数
- getInitParameterNames() 获取所有上下文参数名



2. 注解访问

在对应Servlet类添加 @WebServlet("/url")注解

路径为当前工程下的路径