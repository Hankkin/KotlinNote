## 一、让函数更好调用

### 1.JointoString()
```kotlin
val list = listOf(1,2,3)
fun main(args: Array<String>) {
    print(list)
}
```
假设我们需要输出[1;2;3] 在Java中我们需要用到第三方库，而在Kotlin中。它的标准库中有一个专门的函数来处理这种情况

```kotlin
val list = listOf(1,2,3)

fun main(args: Array<String>) {
    print(joinToString(list,";","(",")"))
}

fun <T> joinToString(collection: Collection<T>,
                     separator: String,
                     prefix: String,
                     postfix: String): String{
    val result = StringBuilder(prefix)
    for ((index,element) in collection.withIndex()){
        if (index>0) result.append(separator)
        result.append(element)
    }
    result.append(postfix)
    return result.toString()
}
```
上面是我们自己实现的JoinToString方法，但是我们看到方法有四个参数，看起来很不方便，下面我们将介绍：

### 2.命名参数

从上面的方法我们看到**joinToString(list,";","(",")")**,看起来对应的参数很麻烦，在Kotlin中，当调用一个Kotlin定义的函数时，可以显示的表明一些参数的名称。例如：

```kotlin
joinToString(collection = list, separator = ";", prefix = "(", postfix = ")")
```
### 3.默认参数值
为什么会出现默认参数值：在Java中，一些类的重载函数实在太多了，只要看一眼java.lang.Thread以及它对应的8个构造方法，就能让人够受的了，这些重载，原本是为了向后兼容，方便这些api的使用者，所以出现了很多重复性。

在Kotlin中，可以在声明函数的时候，指定参数的默认值，这样就可以避免创建重载的函数。

```kotlin
fun <T> joinToString(collection: Collection<T>,
                     separator: String = "，",
                     prefix: String = "",
                     postfix: String = ""): String
```

> 考虑到Java中没有默认参数值的概念，当你从Java中调用Kotlin函数的时候，必须显示的制定所有参数值。如果需要从Java代码中做频繁的调用，而且希望它能对Java的调用者更简便，可以使用**@JvmOverloads**注解它，这个注解指示编译器生成Java重载函数，从最后一个开始省略每个参数