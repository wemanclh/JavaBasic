# 运算
----

## 参数传递

Java 的参数是以值传递的形式传⼊入⽅方法中，⽽而不不是引⽤用传递。

以下代码中 Dog dog 的 dog 是⼀一个指针，存储的是对象的地址。在将⼀一个参数传⼊入⼀一个⽅方法时，本质
上是将对象的地址以值的⽅方式传递到形参中。
```java
public class Dog {
 
    String name;
 
    Dog(String name) {
        this.name = name;
    }
 
    String getName() {
        return this.name;
    }
 
    void setName(String name) {
        this.name = name;
    }
 
    String getObjectAddress() {
        return super.toString();
    }
}
```
在⽅方法中改变对象的字段值会改变原对象该字段值，因为引⽤用的是同⼀一个对象。
```java
class PassByValueExample {
    public static void main(String[] args) {
        Dog dog = new Dog("A");
        func(dog);
        System.out.println(dog.getName());          // B
    }
 
    private static void func(Dog dog) {
        dog.setName("B");
    }
}
```
但是在⽅方法中将指针引⽤用了了其它对象，那么此时⽅方法⾥里里和⽅方法外的两个指针指向了了不不同的对象，在⼀一个
指针改变其所指向对象的内容对另⼀一个指针所指向的对象没有影响。

```java
public class PassByValueExample {
    public static void main(String[] args) {
        Dog dog = new Dog("A");
        System.out.println(dog.getObjectAddress()); // Dog@4554617c
        func(dog);
        System.out.println(dog.getObjectAddress()); // Dog@4554617c
        System.out.println(dog.getName());          // A
    }
 
    private static void func(Dog dog) {
        System.out.println(dog.getObjectAddress()); // Dog@4554617c
        dog = new Dog("B");
        System.out.println(dog.getObjectAddress()); // Dog@74a14482
        System.out.println(dog.getName());          // B
    }
}
```

## float 与 double

ava 不不能隐式执⾏行行向下转型，因为这会使得精度降低。
1.1 字⾯面量量属于 double 类型，不不能直接将 1.1 直接赋值给 float 变量量，因为这是向下转型。 
```java
// float f = 1.1;
```
1.1f 字⾯面量量才是 float 类型。

```java
float f = 1.1f;
```

## 隐式类型转换

因为字⾯面量量 1 是 int 类型，它⽐比 short 类型精度要⾼高，因此不不能隐式地将 int 类型向下转型为 short 类型。
```java
short s1 = 1;
// s1 = s1 + 1;
```
但是使⽤用 += 或者 ++ 运算符会执⾏行行隐式类型转换。
```java
s1 += 1;
s1++;
```
上⾯面的语句句相当于将 s1 + 1 的计算结果进⾏行行了了向下转型：
```java
s1 = (short) (s1 + 1);
```
## switch

从 Java 7 开始，可以在 switch 条件判断语句句中使⽤用 String 对象。
```java
String s = "a";
switch (s) {
    case "a":
        System.out.println("aaa");
        break;
    case "b":
        System.out.println("bbb");
        break;
}
```

switch 不不⽀支持 long、float、double，是因为 switch 的设计初衷是对那些只有少数⼏几个值的类型进⾏行行等
值判断，如果值过于复杂，那么还是⽤用 if ⽐比较合适。

```java
// long x = 111;
// switch (x) { // Incompatible types. Found: 'long', required: 'char, 
byte, short, int, Character, Byte, Short, Integer, String, or an enum'
//     case 111:
//         System.out.println(111);
//         break;
//     case 222:
//         System.out.println(222);
//         break;
// }
```