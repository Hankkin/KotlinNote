## 对象

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