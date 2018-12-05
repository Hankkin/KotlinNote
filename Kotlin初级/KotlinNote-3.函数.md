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

### 4.消除静态工具类：顶层函数和属性

> 在Java中有一些静态的工具类以Util结尾的文件，但是在Kotlin中，根本不需要去创建这些无意义的累。相反，可以把这些函数直接放到代码文件的顶层，不用从属于任何类。这些放在文件顶层的函数依然是包内的成员，如果你需要从包外访问它，则需要import，但不再需要额外包一层。
> 和函数一样，属性也可以放到文件的顶层。

## 二、给别人的类添加方法：扩展函数和属性

```kotlin
package strings
fun String.lastChar(): Char = this.get(this.length-1)
```

在扩展函数中，可以直接访问被扩展的类的其他方法和属性，就好像是在这个类自己的方法中反问他们一样。注意，扩展函数并不允许你打破它的封装行。和在类内部定义的方法不同的是，扩展函数不能访问私有的受保护的成员。

### 1.导入和扩展函数

对于你定义的一个扩展函数，它不会自动的在整个项目范围内生效。相反，如果你要使用它，需要进行导入，就像其他任何类活着函数一样。这是为了避免偶然性的命名冲突。

### 2.从Java中调用扩展函数

实质上，扩展函数就是静态函数，它把调用对象作为了她的第一个参数，调用扩展函数，不会创建适配的对象活着任何运行时的额外消耗。
调用这个静态函数，然后把接收者对象作为第一个参数穿进去即可。和其他顶层函数一样，包含这个函数的Java类的名称，是由这个函数声明的文件名称决定的，假设它声明在一个叫做StringUtils.kt的文件中：char c = StringUtils.lastChar("java")
这个扩展函数被声明为顶层函数，所以他会被编译为一个静态函数在Java中静态导入lastChar 函数，就可以直接使用它了。

### 3.不可重写的扩展函数

扩展函数并不是类的一部分，它是声明类之外的。尽管可以给基类和子类都分别定义一个同名的扩展函数，当这个函数被调用时，它会调用被调用累的函数。

## 三.处理集合：可变参数、中缀调用和库的支持

##四.字符串和正则表达式的处理

## 五.让你的代码更整洁：局部函数和扩展