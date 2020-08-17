## 注解和反射

#### 1.注解

> Annotation是jdk5.0引入的技术



Annotation的作用：对程序做出解释，可以被其他程序使用

Annotation的格式：@注释名(参数)

Annotation的使用：通过反射实现对注解元数据的访问



常见的内置注解：

- @Override：重写，只适用于方法
- @Deprecated:不鼓励程序猿使用，适用于方法、类、属性
- @SuppressWarnings:抑制编译时的警告信息





元注解：负责注解其他注解

- @Target：描述注解使用范围
- @Retention：表示需要在什么级别保存注解，描述注解的生命周期（SOURCE、RUNTIME、CLASS)，RUNTIME>CLASS>SOURCE

- @Documented：表示注解是否生成在javadoc中

- @Inherited：子类可以继承父类注解

  



自定义注解：@interface自定义一个注解

- public @interface 注解名{}
- 每一个方法相当于一个配置参数
- 返回值类型就是参数类型
- 如果只有一个参数，一般为value
- 注解元素必须有值，可以设置默认值





#### 2.反射

Java Reflection

反射机制允许在程序执行期间借助Reflection API获取类的内部信息，

并能直接操作任意对象的内部属性和方法。



核心类：Class

```java
//通过forname获得
Class c1=Class.forName("类名");

//通过对象获得
Class c2=Object.getClass();

//类名.class获得
Class c3=Object.class;

//基本内置类型包装类有TYPE属性
Class c4=Integer.TYPE;

//获得父类类型
Class c5=c1.getSuperClass();

```







反射的优点和缺点：

- 优点：动态创建对象和编译
- 缺点：影响性能







JVM规范定义了三个类加载器：

1. 引导类加载器
2. 扩展类加载器
3. 系统类加载器







获取运行时类的完整结构

```java
        Class c = Class.forName("User");
        //获得类的名字
        System.out.println(c.getName());    //获得包名+类名
        System.out.println(c.getSimpleName());  //获得类名

        System.out.println("===========================");
//        Field[] fields = c.getFields();  //只能找到public属性

        //获得类的属性
        Field[] fields = c.getDeclaredFields();  //可以找到全部属性
        for (Field field : fields) {
            System.out.println(field);
        }
        System.out.println("===========================");
        //获得指定属性的值
        Field declaredField = c.getDeclaredField("name");
        System.out.println("getDeclare：" + declaredField);
        System.out.println("===========================");

        //获得类的方法
        Method[] methods = c.getMethods();  //获得本类及父类的所有public方法
        for (Method method : methods) {
            System.out.println("get：" + method);
        }
        System.out.println("=================================");
        Method[] declaredMethods = c.getDeclaredMethods();  //获得本类的所有方法
        for (Method method : declaredMethods) {
            System.out.println("getDeclare：" + method);
        }

        System.out.println("=================================");
        //获得指定方法
        Method getName = c.getMethod("getName", null);
        Method setName = c.getMethod("setName", String.class);
        System.out.println(getName);
        System.out.println(setName);


        System.out.println("=================================");
        //获得指定的构造器
        Constructor[] constructors = c.getConstructors();
        for (Constructor constructor : constructors) {
            System.out.println("get:"+constructor);
        }
        Constructor[] declaredConstructors = c.getDeclaredConstructors();
        for (Constructor constructor : declaredConstructors) {
            System.out.println("getDeclare:"+constructor);
        }

        System.out.println("=================================");
        Constructor constructor = c.getConstructor();
        System.out.println(constructor);
        Constructor constructor1 = c.getConstructor(String.class, Integer.class, String.class);
        System.out.println(constructor1);

```

通过反射获取对象

```java
        Class c=Class.forName("User");
        User user = (User) c.newInstance();
        System.out.println(user);

        //通过构造器创建对象
        Constructor constructor=c.getDeclaredConstructor(String.class,Integer.class,String.class);
        User user1= (User) constructor.newInstance("zhang",13,"男");
        System.out.println(user1);

        //通过反射调用普通方法
        Method setName = c.getDeclaredMethod("setName", String.class);
        setName.invoke(user1,"new Zhang");
        System.out.println(user1.getName());

        User user2= (User) c.newInstance();
        Field declaredField = c.getDeclaredField("name");
        declaredField.setAccessible(true);  //不能直接操作私有属性
        declaredField.set(user2,"wang");
        System.out.println(user2.getName());

```

