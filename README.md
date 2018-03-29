# Java Scala Kotlin 语言快速切换对照表

## 简介
旨在帮助会这三门语言之一的程序员快速上手其他两种语言，我会在工作中逐渐增加、修缮这一份文档，能力不高，水平有限，难免有错误，请大家提ISSUE或者PR指正！
至于涉及到一些Scala中有但是Java中没有的骚操作，如果是在编译期间实现的（比如implicit/mapReduce），我就不说明了，除非一些简单的（例如操作符重载）可能会说明一下。

平时主要接触的就这三个语言，写Spark用Scala，写Spring Boot 2用Kotlin，会这两门语言怎么能不会Java呢（傲娇脸）。
还有一些JVM平台下的其他优秀语言，也欢迎各位大神PR。


## 数据类型

### 基础类型

|Java|Scala|Kotlin|
|-|-|-|
|int|Int|Int|
|double|Double|Double|
|long|Long|Long|
|float|Float|Float|
|String|String|String|

**说明：** Java的类型有对应的Object的版本（例如`long`->`Long`）,在Scala下，转换的方式为：
```scala
x.asInstanceOf[AnyRef]
```
如果类型已经知道也可以使用比较地味一点的办法：
```scala
java.lang.Long.valueOf(x)
```

### 数组

|项目\语言|Java|Scala|Kotlin|
|-|-|-|-|
|类型表征|T[]|Array[T]|Array<T>|
|初始化|new T[]{a,b,c,d}|Array(a,b,c,d)|arrayOf(a,b,c,d)|

**说明：** Scala和Kotlin中有类型自动推导，不一定要严格的说明类型

### Map
#### 初始化
**Java**
```java
Map<A,B> m = new HashMap<A,B>()
m.put(key1,value1)
m.put(key2,value2)
```
**Scala**
```scala
val m = Map[A,B]("x"->1,"y"->2)
```
**Kotlin**
```kotlin
val m = mapOf("x" to 1,"y" to 2)
```



#### 细节补充
Scala的Map有mutable和immutable之分，immutable的Map（默认就是immutable）不能再加入内容，如果需要初始化一个可以加入内容的Map需要使用`scala.collection.mutable`中的类型。

（未完待续1s）
