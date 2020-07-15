## 抽象

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
