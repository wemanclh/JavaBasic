# 数据类型

## 基本类型
- byte/8
- char/16
- short/16
- int/32
- float/32
- long/64
- double/64
- boolean/~

boolean 只有两个值：true、false，可以使用 1 bit 来存储，但是具体⼤小没有明确规定。JVM 会在编
译时期将 boolean 类型的数据转换为 int，使用 1 来表示 true，0 表示 false。JVM ⽀持 boolean 数组，
但是是通过读写 byte 数组来实现的。

## 包装类型 
基本类型都有对应的包装类型，基本类型与其对应的包装类型之间的赋值使⽤⾃动装箱与拆箱完成。

```java
Integer x = 2;     // 装箱 调⽤用了 Integer.valueOf(2)
int y = x;         // 拆箱 调⽤用了 X.intValue()
```
## 缓存池

new Integer(123) 与 Integer.valueOf(123) 的区别在于：  

- new Integer(123) 每次都会新建⼀个对象；
- Integer.valueOf(123) 会使用缓存池中的对象，多次调⽤用会取得同⼀个对象的引用。  

```java
Integer x = new Integer(123);
Integer y = new Integer(123);
System.out.println(x == y);    // false
Integer z = Integer.valueOf(123);
Integer k = Integer.valueOf(123);
System.out.println(z == k);   // true
```
valueOf() 方法的实现比较简单，就是先判断值是否在缓存池中，如果在的话就直接返回缓存池的内容。

```java
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```

在 java 8 中，Integer 缓存池的⼤⼩默认为 -128~127。

```java
static final int low = -128;
static final int high;
static final Integer cache[];
 
static {
    // high value may be configured by property
    int h = 127;
    String integerCacheHighPropValue =
       
 sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
    if (integerCacheHighPropValue != null) {
        try {
            int i = parseInt(integerCacheHighPropValue);
            i = Math.max(i, 127);
            // Maximum array size is Integer.MAX_VALUE
            h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
        } catch( NumberFormatException nfe) {
            // If the property cannot be parsed into an int, ignore it.
        }
    }
    high = h;
 
    cache = new Integer[(high - low) + 1];
    int j = low;
    for(int k = 0; k < cache.length; k++){
        cache[k] = new Integer(j++);
        
            // range [-128, 127] must be interned (JLS7 5.1.7)
            assert IntegerCache.high >= 127;
    }
```

编译器会在自动装箱过程调⽤用 valueOf() ⽅法，因此多个值相同且值在缓存池范围内的 Integer 实例使用
自动装箱来创建，那么就会引⽤相同的对象。

```java
Integer m = 123;
Integer n = 123;
System.out.println(m == n); // true
```

基本类型对应的缓冲池如下：
- boolean values true and false
- all byte values
- short values between -128 and 127
- int values between -128 and 127
- char in the range \u0000 to \u007F

在使用这些基本类型对应的包装类型时，如果该数值范围在缓冲池范围内，就可以直接使用缓冲池中的对象。

在 jdk 1.8 所有的数值类缓冲池中，Integer 的缓冲池 IntegerCache 很特殊，这个缓冲池的下界是 -128，上界默认是 127，但是这个上界是可调的，在启动 jvm 的时候，通过 -XX:AutoBoxCacheMax=<size> 来指定这个缓冲池的⼤大⼩小，该选项在 JVM 初始化的时候会设定⼀个名为java.lang.IntegerCache.high 系统属性，然后 IntegerCache 初始化的时候就会读取该系统属性来决定上界。


