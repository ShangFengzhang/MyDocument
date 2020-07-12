## 单例模式

#### 一、jdk中的应用  
java.lang.Runtime就是经典的单例模式  
饿汉式

#### 二、使用场景  
单例模式使用的场景：  
1、需要频繁进行创建和销毁的对象  
2、创建或销毁时耗费过多资源的对象  
3、工具类对象、频繁访问数据库或文件的对象


#### 三、代码实现
1.饿汉式（静态常量） 
 
  
    class Singleton {
    //1.构造器私有化
    private Singleton() {
    }
    //2.类内部创建实例对象
    private final static Singleton instance = new Singleton();
    //3.提供共有的静态对象返回方法
    public static Singleton getInstance() {
    return instance;
    }
    }


2.饿汉式（静态代码块）  

    class Singleton {
    //1.构造器私有化
    private Singleton() {
    }
    
    //2.类内部创建实例对象
    private static Singleton instance;
    
    //在静态代码块中创建单例对象
    static {
    instance = new Singleton();
    }
    
    //3.提供共有的静态对象返回方法
    public static Singleton getInstance() {
    return instance;
    }
    }

3.懒汉式（线程不安全）X  

    /*
     * 起到了lazy loading的效果，但是只能在单线程下使用
     * 线程不安全
     * 实际开发中不要使用这种方式*/
    class Singleton {
    private static Singleton instance;
    
    private Singleton() {
    }
    
    //提供一个静态公有方法，当使用该方法时才创建instance
    public static Singleton getInstance() {
    if (instance == null) {
    instance = new Singleton();
    }
    return instance;
    }
    }

4.懒汉式（线程安全，同步类）

    /*
     * 解决了线程不安全问题
     * 效率太低
     * 实际开发中不推荐使用*/
    class Singleton {
    
    private static Singleton instance;
    
    private Singleton() {
    }
    
    //提供一个静态的公有方法，加入同步处理的代码
    public static synchronized Singleton getInstance() {
    if (instance == null) {
    instance = new Singleton();
    }
    return instance;
    }
    }
    
5.懒汉式（线程安全？？？，同步代码块）X 

    /*
     * 这种方法并不能起到线程同步的作用
     * 实际开发中不能使用这种方式*/
    class Singleton {
    
    private static Singleton instance;
    
    private Singleton() {
    }
    
    //提供一个静态的公有方法，加入同步处理的代码
    public static Singleton getInstance() {
    if (instance == null) {
    synchronized (Singleton.class) {
    instance = new Singleton();
    }
    }
    return instance;
    }
    
    }

6.双重检查 ✔  

    /*
     * 推荐使用*/
    class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() {
    }
    
    public static Singleton getInstance() {
    if (instance == null) {
    synchronized (Singleton.class) {
    if (instance == null) {
    instance = new Singleton();
    }
    }
    }
    return instance;
    }
    }

7.静态内部类 ✔     

    /*
     * 推荐使用*/  
    class Singleton {
    private Singleton() {
    }
    
    private static class SingletonInstance {
    private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() {
    return SingletonInstance.INSTANCE;
    }
    }
    
8.枚举 ✔      

     enum Singleton {
    INSTANCE;
    public void sayOK() {
    System.out.println("ok~");
    }
    }
    