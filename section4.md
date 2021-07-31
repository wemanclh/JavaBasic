# 关键字

## final

### 1. 数据  
声明数据为常量量，可以是编译时常量量，也可以是在运⾏行行时被初始化后不不能被改变的常量量。
- 对于基本类型，final 使数值不不变；
- 对于引⽤用类型，final 使引⽤用不不变，也就不不能引⽤用其它对象，但是被引⽤用的对象本身是可以修改的。
```java
final int x = 1;
// x = 2;  // cannot assign value to final variable 'x'
final A y = new A();
y.a = 1;
```

### 2. 方法

声明⽅方法不不能被⼦子类重写。


private ⽅方法隐式地被指定为 final，如果在⼦子类中定义的⽅方法和基类中的⼀一个 private ⽅方法签名相同，此
时⼦子类的⽅方法不不是重写基类⽅方法，⽽而是在⼦子类中定义了了⼀一个新的⽅方法。

### 3. 类  
声明类不不允许被继承。

## static

### 1. 静态变量  
静态变量：⼜又称为类变量量，也就是说这个变量量属于类的，类所有的实例例都共享静态变量量，可以直接
通过类名来访问它。静态变量量在内存中只存在⼀一份。
实例例变量量：每创建⼀一个实例例就会产⽣生⼀一个实例例变量量，它与该实例例同⽣生共死。

```java
public class A {
 
    private int x;         // 实例例变量量
    private static int y;  // 静态变量量
 
    public static void main(String[] args) {
        // int x = A.x;  // Non-static field 'x' cannot be referenced from 
a static context
        A a = new A();
        int x = a.x;
        int y = A.y;
    }
}
```

### 2. 静态⽅法  
静态⽅法在类加载的时候就存在了了，它不不依赖于任何实例例。所以静态⽅方法必须有实现，也就是说它不不能是抽象⽅方法。

```java
public abstract class A {
    public static void func1(){
    }
    // public abstract static void func2();  // Illegal combination of 
modifiers: 'abstract' and 'static'
}
```

只能访问所属类的静态字段和静态⽅方法，⽅方法中不不能有 this 和 super 关键字，因此这两个关键字与具体对象关联。

```java
public class A {
 
    private static int x;
    private int y;
 
    public static void func1(){
        int a = x;
        // int b = y;  // Non-static field 'y' cannot be referenced from a 
static context
        // int b = this.y;     // 'A.this' cannot be referenced from a 
static context
    }
}
```

### 3. 静态语句句块 

静态语句块在类初始化时运⾏一次。

```java
public class A {
    static {
        System.out.println("123");
    }
 
    public static void main(String[] args) {
        A a1 = new A();
        A a2 = new A();
    }
}
```
```java
123
```

### 4. 静态内部类 

⾮非静态内部类依赖于外部类的实例例，也就是说需要先创建外部类实例例，才能⽤用这个实例例去创建⾮非静态内
部类。⽽而静态内部类不不需要。

```java
public class OuterClass {
 
    class InnerClass {
    }
 
    static class StaticInnerClass {
    }
 
    public static void main(String[] args) {
        // InnerClass innerClass = new InnerClass(); // 'OuterClass.this' 
cannot be referenced from a static context
        OuterClass outerClass = new OuterClass();
        InnerClass innerClass = outerClass.new InnerClass();
        StaticInnerClass staticInnerClass = new StaticInnerClass();
    }
}
```
静态内部类不能访问外部类的⾮非静态的变量和⽅法。

### 5. 静态导包 

在使用静态变量和⽅法时不⽤再指明 ClassName，从而简化代码，但可读性⼤大降低

```java
import static com.xxx.ClassName.*
```

### 6. 初始化顺序 

静态变量和静态语句块优先于实例例变量和普通语句句块，静态变量量和静态语句句块的初始化顺序取决于它们
在代码中的顺序。

```java
public static String staticField = "静态变量";
```
```java
static {
    System.out.println("静态语句块");
}
```
```java
public String field = "实例例变量";
```
```java
{
    System.out.println("普通语句句块");
}
```

最后才是构造函数的初始化。

```java
public InitialOrderTest() {
    System.out.println("构造函数");
}
```

存在继承的情况下，初始化顺序为：

- 父类（静态变量、静态语句块）
- 子类（静态变量、静态语句块）
- 父类（实例例变量、普通语句块）
- 父类（构造函数）
- 子类（实例例变量、普通语句块）
- 子类（构造函数）
