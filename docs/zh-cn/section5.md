# Object 通⽤用⽅方法
----

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

I ⾃自反性

```java
x.equals(x); // true
```

## hashCode()
## toString()
## clone()





