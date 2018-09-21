## 一、对象

### 定义
> 用一个类继承另一个类，在子类中增加成员属性或方法，最后创建一个子类的对象

```kotlin
open class One(age: Int) {
    var age: Int =  age
}

class Two: One(age = 10){
    fun add(){
        println()
    }
}

fun main(args: Array<String>) {
    var two = Two()
    two.add()
}
```
通过继承父类进行类扩展，声明的子类可以反复定义新的对象，但是有一个问题，就是如果我还想扩展类，就又要重新声明一个新的子类继承One，又或者是如果我仅需创建扩展类的一个对象，仅为了这么一个对象就声明一个类，太不值得了。解决办法->**对象表达式**

### 对象表达式
> object

```kotlin
open class One(age: Int){
    var age: Int = age
}

fun main(agrs: Array<String>){
    var two = object: One(20){
        fun newAdd(){
            print("我是新增的方法")
        }
    }
    print("${two.age}")
    two.newAdd()
}
```
对象表达式完全可以实现继承的效果

下面看下**对象表达式**的作用
- 增加成员属性和方法
- 覆盖子类或接口的成员属性和方法

例如：
```kotlin
interface Three{
    fun interFunction()
}
open class One(age: Int){
    open var age: Int = age
    open fun classFuction(){
        print("我是类里的方法")
    }
}

fun main(args: Array<String>) {
    var two = object : One(20),Three{
        override fun interFunction() {
            println("在对象表达式中覆盖了接口的方法")
        }

        override var age: Int = 21

        override fun classFuction() {
            println("在对象表达式中覆盖了父类的方法")
        }
    }
    println("覆盖了父类的age属性：${two.age}")
    two.interFunction()
    two.classFuction()
}
```

## 二、类型检查与类型转换

### is !is表达式
> 我们可以在运行是通过上面两个操作符检查一个对象是否是某个特定类：

```kotlin
if(obj is String){
    print(obj.length)
}
if(obj !is String){
    print("Not a String")
}
else{
    print(obj.length)
}
```

###智能转换
> 在很多情形中，需要使用非明确的类型，因为编译器会跟踪**is**检查静态变量，并在需要的时候自动插入安全转换

```kotlin
fun demo(x: Any){
    if(x is String){
        print(x.length)
    }
}
```
编译器足够智能如何转换是安全的，如果不安全将会返回：
```kotlin
if(x !is String) return
print(x.length)//x自动转换为String
```
或者在**|| &&**操作符的右边的值
```kotlin
if(x !is String || x.length == 0) return
if(x is String && x.length > 0){
    print(x.length)
}
```
这样的转换在when表达式和while循环中也会发生
```kotlin
when(x) {
    is Int -> print(x+1)
    is String -> print(x.length)
    is Array<Int> -> print(x.sum())
}
```

###"不安全"的转换符
如果转换是不被允许的那么转换符就会抛出一个异常，因此我们称之为不安全的。在Kotlin中我们用前缀 as操作符
```kotlin
val x: String = y as String
```
注意null不能被转换为String，因为它不是nullable，也就是说如果y是空的，则上面的代码会抛出NPL
为了java的转换语句匹配我们得像下面这样
```kotlin
val x: String? = y as String?
```
### 安全转换符
为了避免抛出异常，可以用as?这个安全转换符，这样失败就会返回null：
```kotlin
val x: String? = y as String?
```
不管as?右边的是不是一个非空String结果都会转换为可空的

## 三、异常

> 所有的异常类都是Exception的子类。每个异常都有一个消息，栈踪迹和可选的原因
使用throw表达式，抛出异常
```kotlin
throw XXException("Hi")
```
使用try捕获异常
```kotlin
try{
    
}catch(e: Exception){
    
}finally{
    
}
```

###try是一个表达式

try可以有返回值：
```kotlin
val a: Int? = try { parseInt(input) } catch (e: NumberFormatException) { null }
```
try返回值要么是try块的最后一个表达式，要么是catch块的最后一个表达式。finally块的内容不会对表达式有任何影响

###检查异常

Kotlin中没有检查异常，这是由多种原因造成的，我们来看个例子：

```kotlin
Appendable append(CharSequence csq) throws IOException;
```

这个签名说了什么？　它说每次我把 string 添加到什么东西(StringBuilder 或者 log console 等等)上时都会捕获 IOExceptions 为什么呢？因为可能涉及到 IO 操作(Writer 也实现了 Appendable)...　所以导致所有实现 Appendable 的接口都得捕获异常

## 四、结构相等与引用相等

- 结构相等  ==
- 引用相等 ===

A.equals(B) 结构相等
引用相等 内存地址相等 一个值赋给另一个值

## 五、this表达式
