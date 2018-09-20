## 万物皆对象
> **对象就是类的实例化**

## 用Kotlin描述对象

```kotlin
class Friend{
    var name: String = ""
    var hairColor: String = ""
    .....
}
```
## 愉快的构造

### 1.主构造函数：
```kotlin
class Friend constructor(val name: String,val age: Int){}
```
### 主构造二
> 下面这种构造纯粹的传值，并不在这里定义属性
> 主构造里不能包含任何代码，Kotlin提供了初始代码块init
```kotlin
class Friend constructor(name: String, age: Int) {
    val name: String = name
    val age: Int = age
    
    init{
        
    }
}
```

### 次构造
> 次构造函数体写在Friend类定义的大括号中，也就是代码块中。而且在次构造函数的定义时，constructor这个关键字是必不可少的。

```kotlin
class Friend {
    val name: String
    val age: Int

    constructor(name: String, age: Int) {
        this.name = name
        this.age = age
    }
}
```
主构造和次构造可以并存么？当然可以。但是如果类有一个主构造函数，那么每个次构造函数都需要委托给主构造函数。也就是说，次构造函数在最后还是要用到主构造函数。

```kotlin
class Friend(name: String,age: Int) {
    val name: String = name
    val age: Int = age
    var parent: Friend? = null

    constructor(name: String, age: Int,child: Friend): this(name,age) {
        child.parent = this
    }
}
```

> **一旦有了修饰符，主构造函数的constructor关键字就不能省略了**

## 属性
### 函数作为属性

```kotlin
class Number(val a: Int, val b: Int, var operator: (Int, Int) -> Int) {
    fun operatoion() {
        println(operator(a, b))
    }
}

fun test() {
    val num = Number(2, 3, { x, y -> x + y })
    num.operatoion()
    num.operator = { x, y -> x * y }
    num.operatoion()
}
```
在这段代码中我们定义的iperator属性为函数类型，（Int，Int）-> Int表示为参数为两个Int类型，返回值为一个Int类型。当然这里没有参数名哦。我们把这个函数属性定义为变量是为了可以日后进行改变以进行来个数的新的操作，在方法operation中，我们执行了这个函数，当然，一个属性能够当成函数来执行是不是很神奇呢？这就是Kotlin函数式编程的魅力！

### gettter()、setter()
```kotlin
class Person(age: Int){
    var age = age
    val isAdult: Boolean
    get() = age >= 18
}
```
get()是自定义getter的标识，当我们从外部需要访问这个属性的值的时候，它会调用内部的getter把值传给我们。

```kotlin
class Person(age: Int){
    var age = age
    val isAdult: Boolean
    get() = age >= 18
    var addAge: Int
    get() = 0
    set(value){
        age += value
    }
}
```
setter是给在外部的属性赋值，通过var的定义，我们可以设置set(value){}自定义setter。当然，如果一个属性要自定义setter，也必须自定义它的getter
