## 一、定义类继承结构

### 1.Kotlin中的接口

如果需要在你的类中实现两个接口会发生什么呢？举个例子：

```kotlin
interface ClickAble {
    fun click()
    fun shhowOff() = print("111")
}

interface ClickAble1 {
    fun click()
    fun shhowOff() = print("222")
}

class A : ClickAble,ClickAble1{
    override fun shhowOff() {

    }


    override fun click() {
    }


}

super.shhowOff<ClickAble1>()
super.shhowOff<ClickAble>()
```

两个接口中均有相同的默认实现的方法，这时候需要你显示的实现该方法，不然会编译错误；如果同样的继承成员有不止一个实现，必须提供一个显示实现，使用尖括号加上父类名字的‘super’表明你想要调用哪个父类的方法。

### 2.open、final、和abstract修饰符：默认为final

在Java中允许你创建任意的子类并重写任意方法，除非显示的使用final进行修饰，也就是说final修饰的父类方法，子类不能重写。对基类进行修改会导致子类不正确的行为，这就是所谓的*脆弱的基类问题*，为了防止这种问题，“要么为继承做好设计并记录文档，要么禁止这么做”。*这意味着所有没有特别需要在子类中被重写的类和方法都要显示的标注为fianl*

在Kotlin中默认为final的，如果你想允许创建一个类的子类，需要使用open修饰符来修饰这个类，此外，需要给每一个可以被重写的属性和方法添加open修饰符。

![WechatIMG52](http://lc-2hxprqvs.cn-n1.lcfile.com/13452df5f69cfa1f4207.jpeg)

注意：如果你重写了一个基类或者接口的成员，重写了的成员同样默认是open的。如果你想改变这一行为，阻止你的子类重写你的实现，可以显示的将重写的成员标注为final。

在Kotlin中，和Java一样 抽象类是不能被实例化的，一个抽象类通常包含一些没有实现并且必须在子类重写的抽象成员。这些成员始终是open的，所以不需要显示的使用open修饰符

![WechatIMG52](http://lc-2hxprqvs.cn-n1.lcfile.com/910b24cb2c47b6d6c102.jpeg)


![WechatIMG52](http://lc-2hxprqvs.cn-n1.lcfile.com/e9bbff8ab718ad16b202.jpeg)


### 3. 可见性修饰符：默认为public

- 1. 总的来说，Kotlin中的可见性修饰符与Java中的类似，同样可以使用public、protected、private，但是默认的可见性是不一样的：如果省略了修饰符，声明就是public的；而Java默认的为private包私有。
- 2. Kotlin中使用internal作为模块内部可见，一个模块就是一组一起编译的Kotlin文件
- 3. Kotlin允许在顶层声明中使用private可见性，包括类、函数和属性

![](http://lc-2hxprqvs.cn-n1.lcfile.com/58a9dcac57d015234b39.jpeg)

### 4.内部类和嵌套类：默认是嵌套类

### 5.密封类sealed

## 二、声明一个带非默认构造方法或属性的类

> 在Java中一个类可以声明一个或多个构造方法。Kotlin中也是类似的，但是区分了主构造方法(通常是主要而简洁的初始化类的方法，并且在类体外部声明)和从构造方法(在类体内部声明)


### 1. 初始化类：主构造和初始化语句块

下面被括号包起来的语句快就叫做主构造方法
```kotlin
class User(val name: String)
```
等同于

```kotlin
class User(name: String){
    val name = name
}
```

```kotlin
class User constructor(name: String){
    val name = name
    
    init {
    }
}

```
*constructor*关键字用来开始一个主构造方法或从构造方法的声明
*init*关键字用来引入一个初始化语句快

上面的声明都是等价的，只不过第一个使用了最简洁的语法

```kotlin
class User(val name: String,val age: Int = 11)
```
> 如果所有的构造方法都有默认值，编译器会生成一个额外的不带参数的构造方法来使用所有的默认值。这可以让Kotlin使用库时变得更简单，因为可以通过无参构造来实例化类

如果你想确保你的类不被其他代码实例化，必须把构造方法标记为private：

