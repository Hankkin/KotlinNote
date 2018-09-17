## Kotlin继承类

### 1.超类(Any)
> 在Kotlin中，所有的类都是继承自**Any**类，这是一个没有父类型的类。即当我们定义各类时，它默认是继承于**Any**类的

例如：
```kotlin
class Person
```
因为**Any**这个类只是给我们提供了equals、hashcode、toString三个方法，我们可以看看Any这个类的源码实现

```kotlin
package kotlin

/**
 * The root of the Kotlin class hierarchy. Every Kotlin class has [Any] as a superclass.
 * 看这个源码注释：意思是任何一个Kotlin的类都继承与这个[Any]类
 */
public open class Any {
    
    // 比较: 在平时的使用中经常用到的equals()函数的源码就在这里额
    public open operator fun equals(other: Any?): Boolean
    
    // hashCode()方法：其作用是返回该对象的哈希值
    public open fun hashCode(): Int
    
    // toString()方法
    public open fun toString(): String
}

```
从源码中我们看出，它直接属于Kotlin这个包下。并且只定义了上面所示的三个方法。在Java中，所有的类默认都是继承于**Object**类的。而**Object**类除了比**Any**多了几个方法和属性外，没有太大区别。但是它们并不是同一个类

从源码中所产生的疑惑：类与函数前面都加了*open*这个修饰符，既然Any是所有类的父类，那么我们自己要定义一个继承类，跟着Any类的语法与结构就能定义一个继承类，所以open修饰符是我们定义继承类的修饰符


### 2.定义
####2.1 继承类的基础使用

定义继承类的关键字为**open**。不管是类、还是成员都需要使用到**open**关键字

```kotlin
open class Person{
    open var name = "Hankkin"
    open fun println()
}
```
例：这里定义一个继承类Demo，并实现两个属性与方法,并且定义一个DemoTest去继承自Demo
```kotlin
open class Demo{

    open var num = 3

    open fun foo() = "foo"

    open fun bar() = "bar"

}

class DemoTest : Demo(){
    // 这里值得注意的是：Kotlin使用继承是使用`:`符号，而Java是使用extends关键字
}

fun main(args: Array<String>) {

    println(DemoTest().num)
    DemoTest().foo()
    DemoTest().bar()

}

```

输入结果为：
```kotlin
3
foo
bar
```
分析：从上面的代码可以看出，DemoTest类只是继承了Demo类，并没有实现任何的代码结构。一样可以使用Demo类中的属性与函数。这就是继承的好处。

#### 2.2继承类的构造函数
- 无主构造函数
> 当实现类无主构造函数时，则每个辅助构造函数必须使用super关键字初始化基类型，或者委托给另一个构造函数。请注意，在这种情况下，不同的辅助构造函数可以调用基类型的不同构造函数

例如：
```kotlin
class MyView : View(){

    constructor(context: Context) : super(context)

    constructor(context: Context, attrs: AttributeSet?) : super(context, attrs)

    constructor(context: Context, attrs: AttributeSet?, defStyleAttr: Int)
        : super(context, attrs, defStyleAttr)
}
```
可以看出，当实现类无主构造函数时，分别使用了super去实现了基类的三个构造函数

- 存在主构造函数
> 当存在主构造函数时，主构造函数一般实现基类型中参数最多的构造函数，参数少的构造函数则用this关键字引用即可

```kotlin
class MyView(context: Context?, attrs: AttributeSet?, defStyleAttr: Int)
    : View(context, attrs, defStyleAttr) {

    constructor(context: Context?) : this(context,null,0)
    
    constructor(context: Context?,attrs: AttributeSet?) : this(context,attrs,0)
}
```

#### 2.3函数的重载与重写
> 在Kotlin中关于函数的重载与重写和Java中几乎是一样的，但是这里还是举例说明一下

- 重写函数中的量点特殊用法
不管是Java还是Kotlin，重写基类型里面的方法，则称为重写，或者是覆盖基类型方法

1.当基类中的函数，没有用到open修饰符修饰的时候，实现类中出现的函数的函数名不能与基类中没有用open修饰符修饰的函数名相同，不管实现类中的函数有没有override修饰符

例如：
```kotlin
open class Demo{
    fun test(){}   // 注意，这个函数没有用open修饰符修饰
}

class DemoTest : Demo(){
    
    // 这里声明一个和基类型无open修饰符修饰的函数，且函数名一致的函数
    // fun test(){}   编辑器直接报红，根本无法运行程序
    // override fun test(){}   同样报红
}
```
2.当一个类不是用open修饰符修饰时，这个类默认就是final的

```kotlin
class A{}

//等价于

final class A{}   // 注意，则的`final`修饰符在编辑器中是灰色的，因为Kotlin中默认的类默认是final的
```
那么当一个基类去继承另外一个基类时，第二个基类不想去覆盖掉第一个基类的方法时，第二个基类的该方法使用final修饰符修饰。

```kotlin
open class A{
    open fun foo(){}
}

// B这个类继承类A，并且类B同样使用open修饰符修饰了的
open class B : Demo(){
   
    // 这里使用final修饰符修饰该方法，禁止覆盖掉类A的foo()函数
    final override fun foo(){}
}
```
- 方法重载

```kotlin
open class Demo{
    open fun foo() = "foo"
}

class DemoTest : Demo(){

    fun foo(str: String) : String{
        return str
    }

    override fun foo(): String {
        return super.foo()
    }
}    

fun main(args: Array<String>) {
    println(DemoTest().foo())
    DemoTest().foo("foo的重载函数")
}
```

#### 2.3重写属性
> -重写属性和重写方法大致是相同的，但是属性不能被重载
> 重写属性即指：在基类中声明的属性，然后在其基类的实现类中重写该属性，该属性必须以override关键字修饰，并且其属性具有和基类中一样的类型。且可以重写该属性的值（getter）

```kotlin
open class Demo{
    open var num = 3
}

class DemoTest : Demo(){
    override var num: Int = 10
}
```

- 重写属性中，val和var的区别
这里可以看出重写了num这个属性，并且为这个属性重写了其值为10，但是，还有一点值得我们去注意：当基类中属性的变量修饰符为val的使用，其实现类可以重写属性可以用var去修饰。反之则不能

```kotlin
open class Demo{
    open val valStr = "我是用val修饰的属性"
}

class DemoTest : Demo(){

    /*
     * 这里用val、或者var重写都是可以的。
     * 不过当用val修饰的时候不能有setter()函数，编辑器直接会报红的
     */
    
    // override val valStr: String
    //   get() = super.valStr

    // override var valStr: String = ""
    //   get() = super.valStr

    // override val valStr: String = ""

    override var valStr: String = "abc"
        set(value){field = value}
}

fun main(arge: Array<String>>){
    println(DemoTest().valStr)

    val demo = DemoTest()
    demo.valStr = "1212121212"
    println(demo.valStr)
}
```

- getter方法慎用super关键字
> 在这里值得注意的是，在实际的项目中在重写属性的时候不用get() = super.xxx,因为这样的话，不管你是否重新为该属性赋了新值，还是支持setter(),在使用的时候都调用的是基类中的属性值。