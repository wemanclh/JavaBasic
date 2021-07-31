# 特性

Java 各版本的新特性
public class Box<T> {
 // T stands for "Type"
 private T t;
 public void set(T t) { this.t = t; }
 public T get() { return t; }
}
### New highlights in Java SE 8
1. Lambda Expressions
2. Pipelines and Streams
3. Date and Time API
4. Default Methods
5. Type Annotations
6. Nashhorn JavaScript Engine
7. Concurrent Accumulators
8. Parallel operations
9. PermGen Error Removed
### New highlights in Java SE 7
1. Strings in Switch Statement
2. Type Inference for Generic Instance Creation
3. Multiple Exception Handling
4. Support for Dynamic Languages
5. Try with Resources
6. Java nio Package
7. Binary Literals, Underscore in literals
8. Diamond Syntax

## Java 与 C++ 的区别
- Java 是纯粹的⾯向对象语⾔，所有的对象都继承⾃ java.lang.Object，C++ 为了兼容 C 即⽀持⾯向对象也⽀持⾯向过程。
- Java 通过虚拟机从⽽实现跨平台特性，但是 C++ 依赖于特定的平台。
- Java 没有指针，它的引⽤可以理解为安全指针，⽽ C++ 具有和 C ⼀样的指针。
- Java ⽀持⾃动垃圾回收，⽽ C++ 需要⼿动回收。
- Java 不⽀持多重继承，只能通过实现多个接⼝来达到相同⽬的，⽽ C++ ⽀持多重继承。
- Java 不⽀持操作符重载，虽然可以对两个 String 对象执⾏加法运算，但是这是语⾔内置⽀持的操作，不属于操作符重载，⽽ C++ 可以。
- Java 的 goto 是保留字，但是不可⽤，C++ 可以使⽤ goto。

## JRE or JDK
- JRE：Java Runtime Environment，Java 运⾏环境的简称，为 Java 的运⾏提供了所需的环境。它是⼀个 JVM 程序，主要包括了 JVM 的标准实现和⼀些 Java 基本类库。
- JDK：Java Development Kit，Java 开发⼯具包，提供了 Java 的开发及运⾏环境。JDK 是 Java 开发的核⼼，集成了 JRE 以及⼀些其它的⼯具，⽐如编译 Java 源码的编译器 javac 等。


