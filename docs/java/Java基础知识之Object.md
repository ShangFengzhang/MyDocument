##  Object类 
 
    Object类是java中所有类的祖先

1. [getClass](#a)
2. [hasCode](#b)
3. [equals](#c)  
	3.1 ["=="与equals](#c_a)  
	3.2 [字符串比较](#c_b)  
	3.3 [重写equals方法时为什么要重写hasCode方法](#c_c)  
4. [clone](#d)
5. [toString](#e)
6. [notify](#f)
7. [notifyAll](#g)
8. [wait](#h)


<h3 id="a"> 1、getClass</h3>
用于返回Class类型的对象

<h3 id="b"> 2、hasCode</h3>
用于哈希查找，获取一个散列值
<h3 id="c"> 3、equals</h3>
比较两个对象是否相等  



<h4 id="c_a">3.1. “==”与equals</h4>  

> “==”比较两个对象的地址是否相等，即判断两个对象是不是同一个对象
> （基本数据类型比较其值，引用数据类型比较内存地址）。equals的作用也是判断两个对象是否相等，但一般有两种使用情况：1、当类没有覆盖equals方法时，等价于“==”；2、通过重写equals方法来比较两个对象“值”是否相等。


<h4 id="c_b"> 3.2. 字符串比较 </h4>  
 
![字符串比较](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/==Andequals.png)   
 
  
<h4 id="c_c"> 3.3. 重写equals方法时为什么要重写hasCode方法  </h4> 

> 两个对象调用equals方法判断相等，那么这两个对象调用hasCode的返回值也相等；反之，两个对象调用的hasCode返回值相等，这两个对象却不一定相等。  
> **hashCode()的默认行为是对堆上的对象产生独特值。如果没有重写 hashCode()，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）**  
> equals方法必须要满足以下几个特性：  
> 1. 自反性，即x.equals(x)==true  
> 2. 对称性，即x.equals(y)==y.equals(x)  
> 3. 传递性，即x.equals(y)==true,y.equals(z)==true，x.equals(z)==true  
> 4. 一致性，如果x对象和y对象有成员变量num1和num2，其中重写的equals只有num1参加了运算，则修改num2不影响x.equals(y)的值
>       
> 此时若某个类没有重写hasCode方法，equals判断两个值相等，但是hasCode不相等，就会造成歧义

<h3 id="d">4、clone</h3>
实现对象的浅拷贝

深拷贝与浅拷贝
浅拷贝是“**虚假的拷贝**”，深拷贝是“**真实的拷贝**”  
> 对于基本数据类型，深拷贝与浅拷贝是一样的，都是拷贝其值。
> 而对于**引用数据类型**，浅拷贝会拷贝其内存地址，当改变这个地址时，会对原来的对象和拷贝的结果同时产生影响。而深拷贝则会拷贝原对象中所有内容，并指向新的内存空间，所以改变其中一个对另一个不会产生影响。
> 深拷贝的速度略慢于浅拷贝并且开销较大。

实现深拷贝的两种方式：  
1、重写clone方法，拿到拷贝后产生的新的对象，然后对新对象的引用类型在调用拷贝操作  
2、通过序列化的方式实现，将要拷贝的对象写入到内存的字节流中，然后再读出刚存储的信息，作为新对象返回

tips：  
a.实现浅拷贝是**覆写**clone方法  
b.序列化：序列化 (Serialization)是将对象的状态信息转换为可以存储或传输的形式的过程。


<h3 id="e">5、toString</h3>

获取对象信息  
包装类中的toString方法是重写过的

<h3 id="f">6、notify</h3>
唤醒该对象上等待的某个线程

<h3 id="g">7、notifyAll</h3>
唤醒在该对象上等待的所有线程

<h3 id="h">8、wait</h3>
使当前线程等待该对象