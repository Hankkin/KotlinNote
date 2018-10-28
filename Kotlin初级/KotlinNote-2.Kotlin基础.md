## 一、函数和变量

### 1.1 语句和表达式
> 语句和表达式的区别在于，表达式有值，并且能作为另一个表达式的一部分使用；而语句总是包围着它的代码块中的顶层元素，并且没有自己的值。在Java中，所有的控制结构都是语句。而在Kotlin中，除了循环(for、while和do/while)以外大多是控制结构都是表达式

### 1.2 表达式函数体

```kotlin
fun max(a: Int,b: Int): int = if(a>b) a else b
```

### 1.3变量

### 1.3.1 可变变量和不可变量

- val （value）不可变量。使用val声明的变量不能在初始化之后再次赋值。它对应的是Java的final变量
- var（variable）可变变量。这种变量的值可以被改变。这种声明对应着是普通的Java变量

> 默认情况下，应该尽可能的使用val关键字来声明所有的kotlin变量，仅在必要额时候换成var

### 1.3.2 字符串模版

```kotlin
println("Hello, $name")
println("Hello, ${name[0]}")
```

## 二、 类和属性

```kotlin
class Person(val name: String)
```
> 在Kotlin中，public是默认的可见性，所以你能省略它

### 2.1 属性
在Java中，字段和其访问器的组合常常被叫做属性，而许多框架验证依赖这个概念。在Kotlin中，属性是头等的语言特性，完全代替了字段和访问器方法。在类中声明一个属性和声明一个变量一样：使用val和var关键字。声明成val的属性是只读的，而var属性是可变的。

### 2.2 自定义访问器

假设你声明这样一个矩形，它能判断自己是否是正方形。不需要一个单独的字段来存储这个信息，因为可以随时检查矩形的长宽是否相等来判断：
```kotlin
class Rectangle(val h: Int,val w: Int){
    val isSquare: Boolean
    get(){
        return h = w
    }
}
```
属性isSquare不需要字段来保存它的值。它只有一个自定义实现的getter。它的值是每次访问属性的时候计算出来的。

### 2.3 目录和包

## 三、 枚举和when

### 3.1 声明枚举类
```kotlin
enum class Color{
    RED,GREEN,YELLOW
}
```

```kotlin
enum class Color(val r: Int,val g: Int, val b: Int){
    RED(255,0,0),GREEN(0,255,0),YELLOW(0,0,255)
}
```
枚举常用的声明构造方法和属性的语法与之前你看到的常规类一样

### 3.2 使用when处理枚举类

```kotlin
fun getMnemonic(color: Color) = when (color) {
    Color.RED -> print("red")
    Color.GREEN -> print("green")
    Color.YELLOW -> print("yellow")
}
```

### 3.3 在when结构中使用任意对象
Kotlin中的when结构比Java中的switch强大的多，switch要求必须使用常量作为分支条件，和它不一样，when允许使用任何对象

### 3.4 使用不带参数的when
```kotlin
fun mixOptimized(c1: Color,c2: Color) = when{
    (c1 == Color.RED && c2 == Color.YELLOW) || (c1 == Color.YELLOW && c2 == Color.RED) -> print("xxx")
    else -> throw Exception("xxx")
}
如果没有给when表达式提供参数，分支条件就是任意的布尔表达式
```

## 四、while循环和for循环

### 4.1 while循环

### 4.2 迭代数字：区间和数列
在Kotlin中没有常规的Java for循环。在这种循环中，先初始化变量，在循环的每一步更新它的值，并在值满足某个限制条件时退出循环。为了代替这种常见的循环，Kotlin使用了区间的概念。
区间本质上就是两个值之间的间隔，这两个值通常是数字：一个起始值。一个结束值，使用..运算符来表示区间
```kotlin
val range = 1..10
```
> 注意Kotlin的区间是包含的或者闭合的，意味着第二个值始终是区间的不部分

```kotlin
for(i in 100 downTo 1 step 2)
```

100 downTo 1是递减的数列  步长为2

## 五、小结

1. fun关键字用来声明函数。val关键字和var关键字分别用来声明只读变量和可变变量
2. 字符串模版帮助你避免繁琐的字符串连接，在变量名出前加上$前缀或者用${}包围一个表达式，来把值注入到字符串中
3. 值对象类在Kotlin中以简洁的方式表示
4. 熟悉的if现在是带返回值的表达式
5. when表达式类似于Java中的switch但功能更强大
6. 在检查过变量具有某种类型之后不必显示的转化它的类型：编译器使用智能转换自动帮你完成
7. for、while、dowhile循环和Java类似，但是for循环现在更加方便，特别是你需要迭代map的时候，又或者是迭代结合需要下表的时候
8. 简洁的语法1..5会创建一个区间。区间和数列允许Kotlin在for循环中使用统一的语法和同一套抽象机制，并且还可以使用in运算符和!in来检查是否属于某个区间
9. Kotlin中的异常处理和Java类似，除了Kotlin不要求你声明函数可以抛出的异常，本章省略...
