## static
1. 修饰类：static不可以用来修饰普通类，只能用来修饰内部类，静态内部类可以直接调用静态构造器（[静态内部类和内部类的区别](https://www.cnblogs.com/aademeng/articles/6192954.html)）
1. 修饰方法：使用static修饰的方法内部不可以调用其他非static方法，被修饰的方法属于这个类，而不是类对应的实例，可以直接使用类名来调用该方法
1. 修饰变量：静态变量随类加载一次，可以被该类的所有对象共享；
1. 修饰代码块：随着类的加载而执行，且只执行一次（和变量一样），执行优先级高于非静态修饰的代码块（这里涉及JVM和Cinit函数知识）（静态代码块->非静态代码块->构造方法），在类初始化执行时执行一次，执行完成后销毁，它仅能初始化类变量，即static修饰的数据成员（用来优化性能）
1. 静态导包：（用来导入类中的静态资源，jdk1.5后的新特性）：格式为import static 这两个关键字连用可以指定导入某个类中的指定静态资源，并且不需要使用类名调用类中静态成员，可以直接使用类中静态成员变量和成员方法。
1. **补充：在一个静态方法里面调用一个非静态成员为什么是非法的？**
   - 由于静态方法不可以通过对象进行调用，因此在静态方法里面，不能调用其他非静态变量，也不可以访问非静态成员
   - **静态的成员属于类，随着类的加载而加载到静态方法区内存（类被虚拟机载入时，会对static变量进行初始化），而类加载时，此时不一定有实例创建，没有实例就不能访问非静态成员。**



## final

1. 修饰类：final修饰类表示这个类不可以被继承，类中所有方法会被隐式声明为fianl
1. 修饰方法：final修饰的方法不可以被重写
1. 修饰变量：如果时基本类型变量，其数值一旦初始化就不可以再修改了，如果是引用类型的变量，则在其初始化在之后不能再让其指向另一个对象。final static修饰变量表示的就是常量。

选择final的原因有两个

   - 把方法锁定，以防继承对该方法进行修改
   - 效率，以前版本的java中final会内嵌调用

---

## this
**this关键字用于引用类的当前实例**。 例如：
```java
class Manager {
    Employees[] employees;
     
    void manageEmployees() {
        int totalEmp = this.employees.length;
        System.out.println("Total employees: " + totalEmp);
        this.report();
    }
     
    void report() { }
}Copy to clipboardErrorCopied
```
在上面的示例中，this关键字用于两个地方：

- this.employees.length：访问类Manager的当前实例的变量。
- this.report（）：调用类Manager的当前实例的方法。

此关键字是可选的，这意味着如果上面的示例在不使用此关键字的情况下表现相同。 但是，使用此关键字可能会使代码更易读或易懂。


## super
super关键字用于从子类访问父类的变量和方法。 例如：
```java
public class Super {
    protected int number;
     
    protected showNumber() {
        System.out.println("number = " + number);
    }
}
 
public class Sub extends Super {
    void bar() {
        super.number = 10;
        super.showNumber();
    }
}Copy to clipboardErrorCopied
```
在上面的例子中，Sub 类访问父类成员变量 number 并调用其其父类 Super 的 `showNumber（）` 方法。
**使用 this 和 super 要注意的问题：**

- 在构造器中使用 `super()` 调用父类中的其他构造方法时，该语句必须处于构造器的首行，否则编译器会报错。另外，this 调用本类中的其他构造方法时，也要放在首行。
- this、super不能用在static方法中。



**简单解释一下：**
被 static 修饰的成员属于类，不属于单个这个类的某个对象，被类中所有对象共享。而 this 代表对本类对象的引用，指向本类对象；而 super 代表对父类对象的引用，指向父类对象；所以， **this和super是属于对象范畴的东西，而静态方法是属于类范畴的东西**。
