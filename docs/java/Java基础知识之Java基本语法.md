## Java基本语法

1. [注释](#a)
2. [标识符使用规范](#b)
3. [Java关键字](#c)
4. [变量和常量](#d)
5. [数据类型](#e)
6. [运算符](#f)
7. [键盘输入](#g)
8. [控制语句](#h)
9. [方法](#i)
10. [枚举](#j)




<h3 id="a"> 1、注释  </h3>  

![三种注释类型](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/note.png)

<h3 id="b"> 2、标识符使用规范  </h3>
 
- 标识符必须以字母、下划线_、美元符号$开头
- 标识符其他部分可以是字母、下划线_、美元符号$和数字的任意组合
- 大小写敏感、长度无限制
- 不可以是Java关键字


**命名**  

有以下几种常见的命名规则：  

<h4>a.驼峰命名法(CamelCase) </h4>    
  
这种命名方式使用大小写混合格式来区别个别单词，并且单词之间不使用空格隔开或连接字符连接  

 1) 大驼峰命名法(UpperCamelCase)	  
 类名需要使用大驼峰命名法 UserAdmin
 
 2) 小驼峰命名法（lowerCameCase）  
 方法名、参数名、成员变量、局部变量需要使用小驼峰命名法 newPassword

<h4>b.蛇形命名法（snake_case）</h4>  
各个单词使用下划线连接,测试方法名、常量、枚举名称使用蛇形命名法，蛇形命名法优势在于单词比较多的时候   set\_password\_test 

<h4>c.串式命名法（kebab-case）</h4>  
各单词使用连接符连接，建议项目文件夹名使用

tips：Java基本命名规范  
1. 包名统一小写，各个单词使用\.分隔符连接，并且单词必须为单数  
2. 测试方法全部小写，常量及枚举名称全部大写  
3. 抽象类名使用Abstract开头  
4. 异常类名使用Exception结尾
5. 测试类名以要测试的类名称开始，Test结尾
6. POJO类中布尔类型的变量不要加is前缀
7. 尽量不要缩写和简写（除非已经被公认）
 
<h3 id="c"> 3、Java关键字 </h3>   
![Java关键字](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/keyword.png)

<h3 id="d">  4、变量和常量  </h3>  
<h4>a.变量的类型 </h4> 

**局部变量**  
方法或语句块内部定义的变量，从声明位置开始到方法或语句块执行完毕为止，使用前必须声明并初始化

**成员变量** 
方法外部、类内部定义的变量，生命周期伴随对象始终，会自动初始化 

**静态变量**
使用static定义，生命周期伴随类始终，会自动初始化

<h4>b.final</h4>  
使用final定义常量

<h3 id="e">   5、数据类型  </h3> 

<h4>a.基本数据类型 </h4>   
<h4>b.引用数据类型（4个字节）</h4>  
![Java关键字](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/dataType.png)

|类型 |存储空间 | 范围 |
|-----|----| ----|
|byte |1字节  |-128~127  |
|short| 2字节 |-2^16~2^16-1 |
|int|4字节|-2^32~2^32-1 |
|long|8字节|-2^64~2^64-1|
|float|4字节|-3.403E38~3.403E38|
|double|8字节|-1.798E308~1.798E308|
|char|2字节|Unicode编码，65536个字符|
|boolean|1bit|true或false|
整型常量默认是int类型，表示long类型在数字末尾加“l”或“L”    
float精确到7位有效数字，double更多一点，浮点型常量默认是double类型，表示float类型在数字末尾加“f”或“F”    
使用BigDecimal进行浮点数比较，或者进行精度计算

自动装箱与自动拆箱  
将基本数据类型转换成对象    
将对象转换成基本数据类型





<h3 id="f"> 6、运算符</h3>  


- 整数运算：一个为Long，结果为Long，否则为int  
- 浮点数运算：一个为double，结果为double，两个都为float结果为float
- 取余%：结果和左操作数符号相同  
- ++a和a++:先赋值和后赋值  
- 关系运算符：char类型也可以比较    
- 逻辑运算符：&&、||的短路情况
- 位运算符：转化成二进制进行运算，左移一位相当于\*2，右移一位相当于/2   （左移n位，*2^n）
- 字符串连接符+：有一个String则结果为String类型
- 条件运算符：x?y:z




**自动类型转换**  
由容量小转换为容量大的（表数范围）  
![Java关键字](https://gitee.com/zhangshangfeng/MyDocument/raw/master/docs/picture/automaticType.png)  
tips：整型值可以直接赋值进行转化    
    
```java
int a=10;  
short b=a;
```

类型转换常见异常：  
溢出；数据丢失（先将一个因子容量进行提升）

<h3 id="g">7、键盘输入 </h3>  
```java
Scanner scanner = new Scanner(System.in);
String a = scanner.nextLine();
Integer b = scanner.nextInt();
System.out.println("a:" + a);
System.out.println("b:" + b);
```

<h3 id="h"> 8、控制语句 </h3>

**1. 顺序结构**  


**2. 选择结构**  
if语句：  
```java
if (a > 0) {
    return true;
}
```

```java
if(a > 0){
	return true;
}else{
	return false;
}
```

```java
if(a > 0){
	return true;
}else if{a < 0}{
	return false;
}else{
	return 0;
}
```

switch语句：  
```java  
switch(a){
case 1:
	System.out.println(1);
	break;
case 2:
	System.out.println(2);
	break;
case 3:
	System.out.println(3);
	break;
default:
	System.out.println(0);
}
```

**3. 循环结构**    

while循环:  
```java
int i=0;
while(i<10){
	i++;
}
```

do-while循环：不管条件是否成立必然执行一次
```java
do{
	i++;
}while(i<10)
```

for循环：    
```java
for(int i=0;i<10;i++){
	System.out.println(i);
}
```

tips:break跳出整个循环；continue跳出本次循环

<h3 id="i"> 9、方法</h3>
java在进行方法调用时都是值传递，即将实参的一个副本传递给形参进行调用。  

重载和重写的区别  
> 重载发生在同一个类中，重写发生在父子类中  
> 重载：方法名相同，参数类型不同、个数不同、顺序不同，返回值和修饰符可以不同  
> 重写：方法名、参数列表、返回值必须相同，异常范围小于等于父类，访问修饰符大于等于父类，private/final/static方法不能重写

<h3 id="j"> 10、枚举</h3>
当需要定义一组常量时可以使用枚举  
尽量不要使用枚举的高级特性



	enum Week {
	   MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
	}