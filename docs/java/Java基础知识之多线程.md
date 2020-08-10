## 多线程

实现多线程有两种常见的方法  
1. 继承Thread类重写run方法,创建Thread对象调用start方法  
2. 实现Runnable接口重写run方法，new一个Thread对象调用start方法  
3. 还有一种方法是实现concurrent包下的Callable接口重写call方法

新生->就绪->运行->阻塞->死亡