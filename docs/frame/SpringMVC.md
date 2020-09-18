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


- default-servlet-handler和@RequestMapping有冲突，加入annotation-driven解决
- resource标签，创建ResourceHttpRequestHandler处理器对象，不依赖服务器
  - mapping指定访问资源的uri，使用通配符“**”
  - location表示静态资源在项目中的位置

6. 解决路径问题

- 使用el表达式
- 使用base标签





7. ssm整合配置

- web.xml

  - 注册DispatcherServlet，创建SpringMVC容器对象，Servlet
  - 注册ContextLoaderListener，创建spring容器对象
  - 注册字符集过滤器，解决乱码问题

  - ``` xml
    <servlet>
        <servlet-name>springDispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
           	<param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springDispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    ```

  - ``` xml
    <context-param>
    	<param-name>contextConfigLocation</param-name>
    	<param-value>classpath:applicationContext.xml</param-value>
    </context-param>
    <listener>
    	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    ```

  - ``` xml
    <filter>
    	<filter-name>CharacterEncodingFilter</filter-name>
    	<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    	<init-param>
    		<param-name>encoding</param-name>
    		<param-value>UTF-8</param-value>
    	</init-param>
    </filter>
    <filter-mapping>
    	<filter-name>CharacterEncodingFilter</filter-name>
    	<url-pattern>/*</url-pattern>
    </filter-mapping>
    ```

- spring配置文件

  - ``` xml
    <!--    引入配置文件-->
    <context:property-placeholder location="classpath:jdbc.properties"/>
    <!-- 连接池 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClass" value="${jdbc.driverClass}"></property>
        <property name="jdbcUrl" value="${jdbc.url}"></property>
        <property name="user" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
    ```

  - ``` xml
    <bean class="org.mybatis.spring.SqlSessionFactoryBean" id="sqlSessionFactoryBean">
        <!-- 指定数据源 -->
        <property name="dataSource" ref="dataSource"></property>
        <!-- MyBatis的配置文件 -->
        <property name="configLocation"
                  value="classpath:mybatis.xml"></property>
        <!-- MyBatis的SQL映射文件 -->
        <property name="mapperLocations"
                  value="classpath:com/gc/mapper/*.xml"></property>
        <!-- 类型别名-->
        <property name="typeAliasesPackage"
                  value="com.gc.bean"></property>
    </bean>
    ```

  - ``` xml
        <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
            <property name="basePackage" value="com.gc.mapper"></property>
        </bean>
    ```

  - ``` xml
    <context:component-scan base-package="com.gc"/>
    ```

- springmvc配置文件

  - ``` xml
    <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/"></property>
        <property name="suffix" value=".jsp"></property>
    </bean>
    ```

  - ``` xml
    <mvc:annotation-driven/> 
    ```

  - ``` xml
    <context:component-scan base-package="main" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
    ```

- mybaits配置文件

- 数据库属性配置文件

  - ``` properties
    jdbc.username=root
    jdbc.password=12345
    jdbc.driverClass=com.mysql.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/database?useSSL=false&useUnicode=true&characterEncoding=utf-8&serverTimezone=Asia/Shanghai
    ```

