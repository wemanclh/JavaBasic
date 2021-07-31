# Object 通⽤用⽅方法

## 概览

```java
public native int hashCode()
 
public boolean equals(Object obj)
 
protected native Object clone() throws CloneNotSupportedException
 
public String toString()
 
public final native Class<?> getClass()
 
protected void finalize() throws Throwable {}
 
public final native void notify()
 
public final native void notifyAll()
 
public final native void wait(long timeout) throws InterruptedException
 
public final void wait(long timeout, int nanos) throws InterruptedException
 
public final void wait() throws InterruptedException
```

## equals()

### 1. 等价关系

两个对象具有等价关系，需要满⾜足以下五个条件：

*I 自反性*

```java
x.equals(x); // true
```

*Ⅱ 对称性*

```java
x.equals(y) == y.equals(x); // true
```

*Ⅲ 传递性*

```java
if (x.equals(y) && y.equals(z))
x.equals(z); // true;
```

*Ⅳ ⼀致性*

多次调⽤用 equals() ⽅方法结果不不变

```java
x.equals(y) == x.equals(y); // true
```

*Ⅴ 与 null 的⽐比较*

对任何不不是 null 的对象 x 调⽤用 x.equals(null) 结果都为 false

```java
x.equals(null); // false;
```

### 2. 等价与相等

- 对于基本类型，== 判断两个值是否相等，基本类型没有 equals() ⽅方法。
- 对于引⽤用类型，== 判断两个变量量是否引⽤用同⼀一个对象，⽽而 equals() 判断引⽤用的对象是否等价。

```java
Integer x = new Integer(1);
Integer y = new Integer(1);
System.out.println(x.equals(y)); // true
System.out.println(x == y); // false
```

### 3. 实现

- 检查是否为同⼀一个对象的引⽤用，如果是直接返回 true；
- 检查是否是同⼀一个类型，如果不不是，直接返回 false；
- 将 Object 对象进⾏行行转型；
- 判断每个关键域是否相等。

```java
public class EqualExample {

    private int x;
    private int y;
    private int z;
    public EqualExample(int x, int y, int z) {
    this.x = x;
    this.y = y;
    this.z = z;
}
@Override
public boolean equals(Object o) {
    if (this == o) return true;
    if (o == null || getClass() != o.getClass()) return false;
    EqualExample that = (EqualExample) o;
    if (x != that.x) return false;
    if (y != that.y) return false;
    return z == that.z;
    }
}
```

## hashCode()

hashCode() 返回哈希值，⽽ equals() 是⽤来判断两个对象是否等价。等价的两个对象散列值⼀定相
同，但是散列值相同的两个对象不⼀定等价，这是因为计算哈希值具有随机性，两个值不同的对象可能计算出相同的哈希值。

在覆盖 equals() ⽅法时应当总是覆盖 hashCode() ⽅法，保证等价的两个对象哈希值也相等。

HashSet 和 HashMap 等集合类使⽤了 hashCode() ⽅法来计算对象应该存储的位置，因此要将对象添加到这些集合类中，需要让对应的类实现 hashCode() ⽅法。

下⾯的代码中，新建了两个等价的对象，并将它们添加到 HashSet 中。我们希望将这两个对象当成⼀样的，只在集合中添加⼀个对象。但是 EqualExample 没有实现hashCode() ⽅法，因此这两个对象的哈希值是不同的，最终导致集合添加了两个等价的对象。

```java
EqualExample e1 = new EqualExample(1, 1, 1);
EqualExample e2 = new EqualExample(1, 1, 1);
System.out.println(e1.equals(e2)); // true
HashSet<EqualExample> set = new HashSet<>();
set.add(e1);
set.add(e2);
System.out.println(set.size()); // 2
```
理想的哈希函数应当具有均匀性，即不相等的对象应当均匀分布到所有可能的哈希值上。这就要求了哈
希函数要把所有域的值都考虑进来。可以将每个域都当成 R 进制的某⼀位，然后组成⼀个 R 进制的整
数。

R ⼀般取 31，因为它是⼀个奇素数，如果是偶数的话，当出现乘法溢出，信息就会丢失，因为与 2 相
乘相当于向左移⼀位，最左边的位丢失。并且⼀个数与 31 相乘可以转换成移位和减法： 31*x ==
(x<<5)-x ，编译器会⾃动进⾏这个优化。
```java
@Override
public int hashCode() {
 int result = 17;
 result = 31 * result + x;
 result = 31 * result + y;
 result = 31 * result + z;
 return result;
}
```

## toString()

默认返回 ToStringExample@4554617c 这种形式，其中 @ 后⾯的数值为散列码的⽆符号⼗六进制表
示。

```java
public class ToStringExample {
 private int number;
 public ToStringExample(int number) {
 this.number = number;
 }
}
```

```java
ToStringExample example = new ToStringExample(123);
System.out.println(example.toString());
```

```java
ToStringExample@4554617c
```

## clone()

### 1. cloneable

clone() 是 Object 的 protected ⽅法，它不是 public，⼀个类不显式去重写 clone()，其它类就不能直接
去调⽤该类实例的 clone() ⽅法。

```java
public class CloneExample {
 private int a;
 private int b;
}
```

```java
CloneExample e1 = new CloneExample();
// CloneExample e2 = e1.clone(); // 'clone()' has protected access in
'java.lang.Object'
```

重写 clone() 得到以下实现：

```java
public class CloneExample {
 private int a;
 private int b;
 @Override
 public CloneExample clone() throws CloneNotSupportedException {
 return (CloneExample)super.clone();
 }
}
```

```java
CloneExample e1 = new CloneExample();
try {
 CloneExample e2 = e1.clone();
} catch (CloneNotSupportedException e) {
 e.printStackTrace();
}
```

```java
java.lang.CloneNotSupportedException: CloneExample
```

以上抛出了 CloneNotSupportedException，这是因为 CloneExample 没有实现 Cloneable 接⼝。

应该注意的是，clone() ⽅法并不是 Cloneable 接⼝的⽅法，⽽是 Object 的⼀个 protected ⽅法。
Cloneable 接⼝只是规定，如果⼀个类没有实现 Cloneable 接⼝⼜调⽤了 clone() ⽅法，就会抛出
CloneNotSupportedException。

```java
public class CloneExample implements Cloneable {
 private int a;
 private int b;
 @Override
 public Object clone() throws CloneNotSupportedException {
 return super.clone();
 }
}
```
### 2. 浅拷⻉

拷⻉对象和原始对象的引⽤类型引⽤同⼀个对象。

```java
public class ShallowCloneExample implements Cloneable {
 private int[] arr;
 public ShallowCloneExample() {
 arr = new int[10];
 for (int i = 0; i < arr.length; i++) {
 arr[i] = i;
 }
 }
 public void set(int index, int value) {
 arr[index] = value;
 }
 public int get(int index) {
 return arr[index];
 }
 @Override
 protected ShallowCloneExample clone() throws CloneNotSupportedException
{
 return (ShallowCloneExample) super.clone();
 }
}
```

```java
ShallowCloneExample e1 = new ShallowCloneExample();
ShallowCloneExample e2 = null;
try {
 e2 = e1.clone();
} catch (CloneNotSupportedException e) {
 e.printStackTrace();
}
e1.set(2, 222);
System.out.println(e2.get(2)); // 222
```

### 3. 深拷⻉ 

拷⻉对象和原始对象的引⽤类型引⽤不同对象。

```java
public class DeepCloneExample implements Cloneable {
 private int[] arr;
 public DeepCloneExample() {
 arr = new int[10];
 for (int i = 0; i < arr.length; i++) {
 arr[i] = i;
 }
 }
 public void set(int index, int value) {
 arr[index] = value;
 }
 public int get(int index) {
 return arr[index];
 }
 @Override
 protected DeepCloneExample clone() throws CloneNotSupportedException {
 DeepCloneExample result = (DeepCloneExample) super.clone();
 result.arr = new int[arr.length];
 for (int i = 0; i < arr.length; i++) {
 result.arr[i] = arr[i];
 }
 return result;
 }
}
```

```java
DeepCloneExample e1 = new DeepCloneExample();
DeepCloneExample e2 = null;
try {
 e2 = e1.clone();
} catch (CloneNotSupportedException e) {
 e.printStackTrace();
}
e1.set(2, 222);
System.out.println(e2.get(2)); // 2
```

### 4. clone() 的替代⽅案 
使⽤ clone() ⽅法来拷⻉⼀个对象即复杂⼜有⻛险，它会抛出异常，并且还需要类型转换。EffectiveJava 书上讲到，最好不要去使⽤ clone()，可以使⽤拷⻉构造函数或者拷⻉⼯⼚来拷⻉⼀个对象。

```java
public class CloneConstructorExample {
 private int[] arr;
 public CloneConstructorExample() {
 arr = new int[10];
 for (int i = 0; i < arr.length; i++) {
 arr[i] = i;
 }
 }
 public CloneConstructorExample(CloneConstructorExample original) {
 arr = new int[original.arr.length];
 for (int i = 0; i < original.arr.length; i++) {
 arr[i] = original.arr[i];
 }
 }
 public void set(int index, int value) {
 arr[index] = value;
 }
 public int get(int index) {
 return arr[index];
 }
}
```

```java
CloneConstructorExample e1 = new CloneConstructorExample();
CloneConstructorExample e2 = new CloneConstructorExample(e1);
e1.set(2, 222);
System.out.println(e2.get(2)); // 2
```
