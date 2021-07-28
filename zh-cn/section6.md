# 继承

## 访问权限
Java 中有三个访问权限修饰符：private、protected 以及 public，如果不加访问修饰符，表示包级可⻅。

可以对类或类中的成员（字段和⽅法）加上访问修饰符。
- 类可⻅表示其它类可以⽤这个类创建实例对象。
- 成员可⻅表示其它类可以⽤这个类的实例对象访问到该成员；
  
protected ⽤于修饰成员，表示在继承体系中成员对于⼦类可⻅，但是这个访问修饰符对于类没有意义。

设计良好的模块会隐藏所有的实现细节，把它的 API 与它的实现清晰地隔离开来。模块之间只通过它们的 API 进⾏通信，⼀个模块不需要知道其他模块的内部⼯作情况，这个概念被称为信息隐藏或封装。因此访问权限应当尽可能地使每个类或者成员不被外界访问。

如果⼦类的⽅法重写了⽗类的⽅法，那么⼦类中该⽅法的访问级别不允许低于⽗类的访问级别。这是为了确保可以使⽤⽗类实例的地⽅都可以使⽤⼦类实例去代替，也就是确保满⾜⾥⽒替换原则。
字段决不能是公有的，因为这么做的话就失去了对这个字段修改⾏为的控制，客户端可以对其随意修改。例如下⾯的例⼦中，AccessExample 拥有 id 公有字段，如果在某个时刻，我们想要使⽤ int 存储id 字段，那么就需要修改所有的客户端代码。

```java
public class AccessExample {
 public String id;
}
```
可以使⽤公有的 getter 和 setter ⽅法来替换公有字段，这样的话就可以控制对字段的修改⾏为。
```java
public class AccessExample {
 private int id;
 public String getId() {
 return id + "";
 }
 public void setId(String id) {
 this.id = Integer.valueOf(id);
 }
}
```
但是也有例外，如果是包级私有的类或者私有的嵌套类，那么直接暴露成员不会有特别⼤的影响。
```java
public class AccessWithInnerClassExample {
 private class InnerClass {
 int x;
 }
 private InnerClass innerClass;
 public AccessWithInnerClassExample() {
 innerClass = new InnerClass();
 }
 public int getValue() {
 return innerClass.x; // 直接访问
 }
}
```
## 抽象类与接⼝口
### 1. 抽象类
抽象类和抽象⽅法都使⽤ abstract 关键字进⾏声明。如果⼀个类中包含抽象⽅法，那么这个类必须声明为抽象类。

抽象类和普通类最⼤的区别是，抽象类不能被实例化，只能被继承。
```java
public abstract class AbstractClassExample {
 protected int x;
 private int y;
 public abstract void func1();
 public void func2() {
 System.out.println("func2");
 }
}
```
```java
public class AbstractExtendClassExample extends AbstractClassExample {
 @Override
 public void func1() {
 System.out.println("func1");
 }
}
// AbstractClassExample ac1 = new AbstractClassExample(); //
'AbstractClassExample' is abstract; cannot be instantiated
AbstractClassExample ac2 = new AbstractExtendClassExample();
ac2.func1();
```
### 2. 接⼝ 
接⼝是抽象类的延伸，在 Java 8 之前，它可以看成是⼀个完全抽象的类，也就是说它不能有任何的⽅法实现。

从 Java 8 开始，接⼝也可以拥有默认的⽅法实现，这是因为不⽀持默认⽅法的接⼝的维护成本太⾼了。在 Java 8 之前，如果⼀个接⼝想要添加新的⽅法，那么要修改所有实现了该接⼝的类，让它们都实现新增的⽅法。

接⼝的成员（字段 + ⽅法）默认都是 public 的，并且不允许定义为 private 或者 protected。从 Java 9开始，允许将⽅法定义为 private，这样就能定义某些复⽤的代码⼜不会把⽅法暴露出去。

接⼝的字段默认都是 static 和 final 的。
```java
public interface InterfaceExample {
 void func1();
 default void func2(){
 System.out.println("func2");
 }
 int x = 123;
 // int y; // Variable 'y' might not have been initialized
 public int z = 0; // Modifier 'public' is redundant for interface
fields
 // private int k = 0; // Modifier 'private' not allowed here
 // protected int l = 0; // Modifier 'protected' not allowed here
 // private void fun3(); // Modifier 'private' not allowed here
}
```
```java
public class InterfaceImplementExample implements InterfaceExample {
 @Override
 public void func1() {
 System.out.println("func1");
 }
}
// InterfaceExample ie1 = new InterfaceExample(); // 'InterfaceExample' is
abstract; cannot be instantiated
InterfaceExample ie2 = new InterfaceImplementExample();
ie2.func1();
System.out.println(InterfaceExample.x);
```

### 3. ⽐较 
- 从设计层⾯上看，抽象类提供了⼀种 IS-A 关系，需要满⾜⾥式替换原则，即⼦类对象必须能够替换掉所有⽗类对象。⽽接⼝更像是⼀种 LIKE-A 关系，它只是提供⼀种⽅法实现契约，并不要求接⼝和实现接⼝的类具有 IS-A 关系。
- 从使⽤上来看，⼀个类可以实现多个接⼝，但是不能继承多个抽象类。
- 接⼝的字段只能是 static 和 final 类型的，⽽抽象类的字段没有这种限制。
- 接⼝的成员只能是 public 的，⽽抽象类的成员可以有多种访问权限。
### 4. 使⽤选择 

*使⽤接⼝：*
需要让不相关的类都实现⼀个⽅法，例如不相关的类都可以实现 Comparable 接⼝中的compareTo() ⽅法；
需要使⽤多重继承。

*使⽤抽象类：*
- 需要在⼏个相关的类中共享代码。
- 需要能控制继承来的成员的访问权限，⽽不是都为 public。
- 需要继承⾮静态和⾮常量字段。

在很多情况下，接⼝优先于抽象类。因为接⼝没有抽象类严格的类层次结构要求，可以灵活地为⼀个类添加⾏为。并且从 Java 8 开始，接⼝也可以有默认的⽅法实现，使得修改接⼝的成本也变的很低。
## super

- 访问⽗类的构造函数：可以使⽤ super() 函数访问⽗类的构造函数，从⽽委托⽗类完成⼀些初始化的⼯作。应该注意到，⼦类⼀定会调⽤⽗类的构造函数来完成初始化⼯作，⼀般是调⽤⽗类的默认构造函数，如果⼦类需要调⽤⽗类其它构造函数，那么就可以使⽤ super() 函数。

- 访问⽗类的成员：如果⼦类重写了⽗类的某个⽅法，可以通过使⽤ super 关键字来引⽤⽗类的⽅法实现。

```java
public class SuperExample {
 protected int x;
 protected int y;
 public SuperExample(int x, int y) {
 this.x = x;
 this.y = y;
 }
 public void func() {
 System.out.println("SuperExample.func()");
 }
}
```

```java
public class SuperExtendExample extends SuperExample {
 private int z;
 public SuperExtendExample(int x, int y, int z) {
 super(x, y);
 this.z = z;
 }
 @Override
 public void func() {
 super.func();
 System.out.println("SuperExtendExample.func()");
 }
}
```
```java
SuperExample e = new SuperExtendExample(1, 2, 3);
e.func();
```
```java
SuperExample.func()
SuperExtendExample.func()
```

## 重写与重载
### 1. 重写（Override） 
存在于继承体系中，指⼦类实现了⼀个与⽗类在⽅法声明上完全相同的⼀个⽅法。

为了满⾜⾥式替换原则，重写有以下三个限制：
- ⼦类⽅法的访问权限必须⼤于等于⽗类⽅法；
- ⼦类⽅法的返回类型必须是⽗类⽅法返回类型或为其⼦类型。
- ⼦类⽅法抛出的异常类型必须是⽗类抛出异常类型或为其⼦类型。

使⽤ @Override 注解，可以让编译器帮忙检查是否满⾜上⾯的三个限制条件。

下⾯的示例中，SubClass 为 SuperClass 的⼦类，SubClass 重写了 SuperClass 的 func() ⽅法。其中：

- ⼦类⽅法访问权限为 public，⼤于⽗类的 protected。
- ⼦类的返回类型为 ArrayList<Integer>，是⽗类返回类型 List<Integer> 的⼦类。
- ⼦类抛出的异常类型为 Exception，是⽗类抛出异常 Throwable 的⼦类。
- ⼦类重写⽅法使⽤ @Override 注解，从⽽让编译器⾃动检查是否满⾜限制条件。

```java
class SuperClass {
 protected List<Integer> func() throws Throwable {
 return new ArrayList<>();
 }
}
class SubClass extends SuperClass {
 @Override
 public ArrayList<Integer> func() throws Exception {
 return new ArrayList<>();
 }
}
```
在调⽤⼀个⽅法时，先从本类中查找看是否有对应的⽅法，如果没有再到⽗类中查看，看是否从⽗类继承来。否则就要对参数进⾏转型，转成⽗类之后看是否有对应的⽅法。总的来说，⽅法调⽤的优先级为：

- this.func(this)
- super.func(this)
- this.func(super)
- super.func(super)

```java
/*
 A
 |
 B
 |
 C
 |
 D
*/
class A {
 public void show(A obj) {
 System.out.println("A.show(A)");
 }
 public void show(C obj) {
 System.out.println("A.show(C)");
 }
}
class B extends A {
 @Override
 public void show(A obj) {
 System.out.println("B.show(A)");
 }
}
class C extends B {
}
class D extends C {
}
```
```java
public static void main(String[] args) {
 A a = new A();
 B b = new B();
 C c = new C();
 D d = new D();
 // 在 A 中存在 show(A obj)，直接调⽤
 a.show(a); // A.show(A)
 // 在 A 中不存在 show(B obj)，将 B 转型成其⽗类 A
 a.show(b); // A.show(A)
 // 在 B 中存在从 A 继承来的 show(C obj)，直接调⽤
 b.show(c); // A.show(C)
 // 在 B 中不存在 show(D obj)，但是存在从 A 继承来的 show(C obj)，将 D 转型成其
⽗类 C
 b.show(d); // A.show(C)
 // 引⽤的还是 B 对象，所以 ba 和 b 的调⽤结果⼀样
 A ba = new B();
 ba.show(c); // A.show(C)
 ba.show(d); // A.show(C)
}
 ```
### 2. 重载（Overload）
存在于同⼀个类中，指⼀个⽅法与已经存在的⽅法名称上相同，但是参数类型、个数、顺序⾄少有⼀个不同。

应该注意的是，返回值不同，其它都相同不算是重载。 

```java
class OverloadingExample {
 public void show(int x) {
 System.out.println(x);
 }
 public void show(int x, String y) {
 System.out.println(x + " " + y);
 }
}
```

```java
public static void main(String[] args) {
 OverloadingExample example = new OverloadingExample();
 example.show(1);
 example.show(1, "2");
}
```
