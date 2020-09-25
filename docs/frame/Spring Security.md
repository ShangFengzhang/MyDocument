## Spring Security

> 认证：通过用户名与密码成功登录系统后，让系统得到当前用户的角色信息
>
> 授权：系统根据当前用户的角色，给其授予相对应的权限资源



### 1.Spring Security概念

Spring Security是基于SpringAOP思想，Servlet过滤器实现的安全框架

Spring Security认证流程：

![SpringSecurity_1](D:\Git\BLOGS\docs\picture\SpringSecurity_1.png)

用户发送请求以后，会进入UsernamePasswordAuthenticationFilter过滤器获取用户名和密码，这个过滤器限制了请求方式必须为POST

``` java
  public UsernamePasswordAuthenticationFilter() {
        super(new AntPathRequestMatcher("/login", "POST"));
    }
```

``` java
    //只接受post请求
    if (this.postOnly && !request.getMethod().equals("POST")) {
        throw new AuthenticationServiceException("Authentication method not supported: " + request.getMethod());
```

``` java
	//获取username
    String username = this.obtainUsername(request);
    //获取username
    String password = this.obtainPassword(request);
    //防止空指针异常
    if (username == null) {
        username = "";
    }

    if (password == null) {
        password = "";
    }

    username = username.trim();
```

然后构建一个UsernamePasswordAuthenticationToken对象

``` java
  UsernamePasswordAuthenticationToken authRequest = new UsernamePasswordAuthenticationToken(username, password);
```

此时还没有进行认证，调用UsernamePasswordAuthenticationToken的第一个构造方法

``` java
    public UsernamePasswordAuthenticationToken(Object principal, Object credentials) {
        super((Collection)null);
        this.principal = principal;
        this.credentials = credentials;
        this.setAuthenticated(false);
    }
```

之后将请求的实际信息存入UsernamePasswordAuthenticationToken对象中

``` java
    //将请求信息设置到UsernamePasswordAuthenticationToken中
    this.setDetails(request, authRequest);
```

然后调用AuthenticationManager

``` java
   return this.getAuthenticationManager().authenticate(authRequest);
```

getAuthenticationManager返回一个AuthenticationManager对象

``` java
    protected AuthenticationManager getAuthenticationManager() {
        return this.authenticationManager;
    }
```

AuthenticationManager是一个接口，里面封装了authenticate方法

``` java
public interface AuthenticationManager {
    Authentication authenticate(Authentication var1) throws AuthenticationException;
}
```

ProviderManager实现了这个接口，在authenticate方法中，通过循环根据不同的认证方式（用户名密码、微信等）挑选不同的provider，然后调用authenticate方法

``` java
  result = provider.authenticate(authentication);
```

authenticate方法在AuthenticationProvider接口中，AbstractUserDetailsAuthenticationProvider实现了这个接口，在authenticate方法中通过调用retrieveUser方法获取了一个user对象，这个对象就是我们需要的用户

```java
user = this.retrieveUser(username, (UsernamePasswordAuthenticationToken)authentication);
```

retrieveUser是一个抽象方法，他的实现在DaoAuthenticationProvider抽象类中，返回一个UserDetails对象

``` java
    UserDetails loadedUser = this.getUserDetailsService().loadUserByUsername(username);
```

根据loadUserByUsername获取user对象，所以我们只需要重写UserDetailsService接口，实现这个方法，就可以使用数据库中的数据实现认证了





### 2.Spring Security常用包

- spring-security-core.jar  核心包
- spring-secutiry-web.jar web包
- spring-secutiry-config.jar 配置包
- spring-secutiry-taglibs.jar 动态标签包

### 3. web.xml配置

``` xml
<filter>
	<filter-name>springSecurityFilterChain</filter-name>
    <filter-class>org.springframework.web.filter.DelegatingFilterProxy</filter-class>
</filter>
<filter-mapping>
	<filter-name>springSecurityFilterChain</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 4. Spring Security配置

``` xml
<!--释放静态资源-->
<security:http pattern="/css/**" security="none"/>
<!--
auto-config="true" 自动加载Spring Security配置文件
use-expressions="true" 使用Spring的el表达式来配置Spring Security
-->
<security:http auto-config="true" use-expressions="true">
    <!--让认证页面可以匿名访问-->
    <security:intercept-url pattern="/login.jsp" access="permitAll()"/>
    <!--拦截资源-->
    <!--
    pattern="/**" 拦截所有资源
	access="hasAnyRole('ROLE_USER')"  
	-->
    <security:intercept-url pattern="/**" access="hasAnyRole('ROLE_USER')"/>
    <!--
	配置认证信息
	-->
    <security:form-login login-page="/login.jsp"
                         login-processing-url="/login"
                         default-target-url="/index.jsp"
                         authentication-failure-url="/faile.jsp"
                         username-parameter="指定用户名"
                         password-parameter="指定密码"/>
    <!--配置登出信息-->
    <security:logout logout-url="/logout"
                     logout-success-url="/login.jsp"/>
    <!--去掉csrf拦截的过滤器-->
    <security:csrf disabled="true"/>
</security:http>
<!--把加密对象放到IOC容器中-->
<bean id="passwordEncoder" class="org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder">
<!--设置认证用户信息来源-->
<!--
{noop}Spring Security默认的认证必须是加密的，加上{noop}表示不加密认证
-->
<security:authentication-manager>
    <security:authentication-provider>
    	<security:user-service>
        	<security:user name="user" password="{noop}user"
                           authorities="ROLE_USER"/>
            <security:user name="admin" password="{noop}admin"
                           authorities="ROLE_ADMIN"/>
        </security:user-service>
    </security:authentication-provider>
    <security:authentication-provider user-service-ref="userServiceImpl">		     	    	<security:password-encoder ref="passwordEncoder"/>
    </security:authentication-provider>
</security:authentication-manager>
```

### 5.Spring Security过滤器

1. org.springframework.security.web.context.SecurityContextPersistenceFilter  

   我称之为基础过滤器，从session中获取SecurityContext，放到上下文中，之后的filter大多依赖这个来获取登录状态

2. org.springframework.security.web.context.WebAsyncManagerIntegrationFilter

   将SecurityContext集成到Spring异步执行机制中的WebAsyncManager

3. org.springframework.security.web.header.HeaderWriterFilter

   想请求的Header中添加相应的信息，可在http标签内部使用security:headers来控制（jsp页面）

4. org.springframework.security.web.csrf.CsrfFilter

   验证请求是否包含csrf的token信息，防止csrf攻击

5. org.springframework.security.web.authentication.logout.LogoutFilter

   匹配URL为/logout的请求，实现用户退出，清除认证信息

6. org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter

   实现认证操作过滤器，默认匹配URL为/login且必须为POST请求

7. org.springframework.security.web.authentication.ui.DeafultLoginPageGeneratingFilter

   如果没有在配置文件中指定认证页面，则由该过滤器生成一个默认认证页面

8. org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter

   产生一个默认的退出登录页面

9. org.springframework.security.web.authentication.www.BasicAuthenticationFilter

   自动解析HTTP请求中头部名字为Authentication,切以Basic开头的头信息

10. org.springframework.security.web.savedrequest.RequestCacheAwareFilter

    通过HttpSessionRequestCache内部维护了一个RequestCache，用于缓存HttpServletRequest

11. org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter

    使Request具有更丰富的API

12. org.springframework.security.web.authentication.AnonymousAuthenticationFilter

    当SecurityContextHolder中认证信息为空，会创建一个匿名用户存入SecurityContextHolder中，

    兼容未登录的访问

13. org.springframework.security.web.session.SessionManagementFilter

    限制开启会话的数量

14. org.springframework.security.web.access.ExceptionTranslationFilter

    异常转换过滤器，用于转换过滤器链中的异常

15. org.springframework.security.web.access.intercept.FilterSecurityInterceptor

    获取所配置资源访问的授权信息，决定用户是否有访问权限

### 6. Spring Security使用自定义认证页面

在xml配置文件中添加 security:form-login 标签

``` xml
  <!--
	配置认证信息
	-->
    <security:form-login login-page="/login.jsp"
                         login-processing-url="/login"
                         default-target-url="/index.jsp"
                         authentication-failure-url="/faile.jsp"
                         username-parameter="指定用户名"
                         password-parameter="指定密码"/>
```

1. 认证界面不配置匿名，过滤器拦截所有资源时访问无法跳转（403）

   ``` xml
    <!--让认证页面可以匿名访问-->
       <security:intercept-url pattern="/login.jsp" access="permitAll()"/>
   ```

2. 静态资源不释放在认证完成后会失效

   ``` xml
   <!--释放静态资源-->
   <security:http pattern="/css/**" security="none"/>
   <security:http pattern="/img/**" security="none"/>
   <security:http pattern="/plugins/**" security="none"/>
   ```

### 7.CSRF

在jsp页面引入Security动态标签 

``` jsp
<%@taglib uri="http://www.springframework.org/security/tags" prefix="security"%>
```

在form表单内引入标签

``` jsp
<security:csrfInput/> <!--表单-->
<security:crsfMetaTags/> <!--ajax-->
```

关闭csrf拦截：

``` xml
    <!--去掉csrf拦截的过滤器-->
    <security:csrf disabled="true"/>
```



### 8.User（org.springframework.security.core.userdetails.User）

Spring Security中的User对象除了封装了用户名和密码，还有另外四个布尔值：

- enabled 是否可用（状态）
- accountNonExpired 账户是否失效（用户名）
- credentialsNonExpired 密码是否失效（密码过期）
- accountNonLocked 账户是否锁定

### 9. Remember Me

- 提供单选或复选框，name属性设置为remember-me，值为 ture、on、yes、1中的一种

- 手动开启remember me过滤器，设置过期时间

  ``` xml
  <security:remember-me token-validity-seconds="60"/>
  ```

- 持久化remember数据

  - 创建表persistent_logins(username,series,token,last_used)

  - 修改配置信息

    ``` xml
    <security:remember-me data-source-ref="dataSource"
                          token-validity-seconds="60"	
                          remember-me-parameter="remember-me"	
                          />
    <!--
    data-source-ref="dataSource" 数据源
    token-validity-seconds="60"	过期时间
    remember-me-parameter="remember-me" 指定参数名
    -->
    ```

- 显示认证后的名字

  - 传统方法

    ``` java
    	 //后台获取认证后的用户名字，放入Request域中
         SecurityContextHolder.getContext().getAuthentication().getName();
     	 (User)SecurityContextHolder.getContext().getAuthentication().getPrincipal();
    ```

  - 动态标签

    ``` xml
    <security:authentication property="principal.username"/>
    <security:authentication property="name"/>
    ```

### 10.权限控制

- 标签控制菜单的显隐

  - jsp

    ``` xml
    <security:authorize access="hasAnyRole("ROLE1","ROLE2")"/>
    ```

- 权限控制

  - 开启权限注解支持

    ``` xml
    <security:global-method-security
            secured-annotations="enabled"  SpringSecurity内部的权限注解
            per-post-annotations="enabled"	Spring指定的权限控制注解
            jsr250-annotations="enabled"    java250注解
     />
    ```

    ``` java
    @Secured({"ROLE_1","ROLE_2"})	//security
    @RoleAllowed({"ROLE_1","ROLE_2"}) //jsr250
    @PreAuthorize("hasAnyAuthority('ROLE_1','ROLE_2')")	//spring的el表达式注解
    ```

### 11.异常处理

- 在配置文件中处理

  ``` xml
  <security:access-denied-handler error-page="/403.jsp"/>
  <!--只能处理403异常-->
  ```

  

