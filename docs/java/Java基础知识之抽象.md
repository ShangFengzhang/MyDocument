## 抽象

<h3> 抽象类和抽象方法  </h3>
抽象类和抽象方法使用abstract关键字修饰  
有抽象方法的类必须定义成抽象类，不能使用new关键字进行实例化， 
抽象类是为子类提供的模版，只能用来被子类继承     
tips：抽象类可以包含属性、方法、构造方法，但是**构造方法只能呗子类调用**  
抽象方法没有实现，子类必须实现抽象方法

    abstract class Animal {
        abstract public void shout();
    }

    class Dog extends Animal {

        @Override
        public void shout() {
            System.out.println("汪汪汪");
        }
    }


<h3> 接口 </h3>
 
接口使用interface关键字修饰  

- 接口的访问修饰符只能是public或默认
- 接口可以多继承
- 接口中只能定义常量  （public static final）
- 接口中的方法默认是抽象方法 （public abstract）
- 子类必须实现接口中的所有方法

jdk1.8新特性：在jdk8环境中，接口中不再是只能有抽象方法，可以有静态方法和default方法


<h3> 内部类 </h3>

<h5> 1.成员内部类（可以使用任意修饰符修饰） </h5>  
<h6> a)非静态内部类 </h6>   

非静态内部类可以直接访问外部类成员  
非静态内部类必须及存在一个外部类对象里  
非静态内部类不能有静态方法、静态属性和静态初始化块  
外部类的静态方法、静态代码块不能访问非静态内部类 
 
<h6> b）静态内部类 </h6> 
<h5> 2.匿名内部类 </h5>
适合只使用一次的类


<h5> 3.局部内部类 </h5>

定义在方法中的类