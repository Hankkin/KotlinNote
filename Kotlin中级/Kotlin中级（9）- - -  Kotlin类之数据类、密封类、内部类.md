## 数据类

### 1.声明
> **data**关键字
```kotlin
data class Leaf(val size: String,val color: String,val shape: String,val vein: Int)
```

### 2.数据类声明条件
1. 主构造函数最少要有一个参数
2. 数据类的主构造器的所有参数必须标记为val或var
3. 数据类不能是抽象类、open类、封闭类、内部类
4. 数据类不能继承自任何其他类（但可以实现接口）

### 3.访问数据类的2种方法
1. 和普通类一样"对象名.数据名"
2. 编译器从主构造函数中声明的属性中导出的成员方法componentN()函数群

```kotlin
data class Leaf(val size: String,val color: String,val shape: String,val vein: Int)

fun main(args: Array<String>) {
    val leaf = Leaf("20","green","cicle",57)
    val size = leaf.size
    val color = leaf.color
    val shape = leaf.shape
    val vein = leaf.vein
    println("大小：$size 颜色： $color 形状 $shape 叶子数： $vein")
}
```

```kotlin
data class Leaf(val size: String,val color: String,val shape: String,val vein: Int)

fun main(args: Array<String>) {
    val leaf = Leaf("20","green","cicle",57)
    val(size,color,shape,vein) = leaf
    println("大小：$size 颜色： $color 形状 $shape 叶子数： $vein")
}
```

### 4.componentN()函数群
> componentN函数群会按声明顺序对应于所有属性

上面的代码我们可以看到结构出来的变脸可以直接拿来用，比如数据体Leaf中的size属性，componentN函数群会按照数据体Leaf中属性声明的顺序，从component1到component4和size、color、shape、及vein一一对应。

### 5.编辑器做的事
1. 生成equals()函数与hasCode()函数
2. 生成toString()函数，由类名（参数1 = 值1，参数2 = 值2，....）构成
3. 由所定义的属性自动生成component1()、component2()、...、componentN()函数，其对应于属性的声明顺序。
4. copy()函数。（Koltin要修改数据类的属性，则使用其独有的copy()函数。其作用就是：修改部分属性，但是保持其他不变）


## 密封类

### 1. 什么是受限的类继承结构
> 1. 所谓受限的类继承结构，即当类中的一个值只能是有限的几种类型，而不能是其他的任何类型。
> 2. 这种受限的类继承结构从某种意义上讲，它相当于是枚举类的扩展。但是，我们知道Kotlin的枚举类中的枚举常量是受限的，因为每一个枚举常量只能存在一个实例
> 3. 但是其和枚举类不同的地方在于，密封类的一个子类可以有可包含状态的多个实例。
> 4. 也可以说成，密封类是包含了一组受限的类集合，因为里面的类都是继承自这个密封类的。但是其和其他继承类（open）的区别在，密封类可以不被此文件外被继承，有效保护代码。但是，其密封类的子类的扩展是是可以在程序中任何位置的，即可以不在同一文件下

### 2.声明
```kotlin
sealed class SealedExpr()
```

## 内部类（嵌套类）
> 在实际开发中，用到内部类的地方是很多的。比如说：
> - 对于Android开发来说，列表适配器中的ViewHolder类就是一个内部类
> - 根据后台开发人员提供的json字符串生成的对象中，也包含另外一个对象，这也是一个内部类

### 1.嵌套类
上面提到的两种情况，是在开发中最常见的。当然说到内部类，就必须提到另一个概念，嵌套类，所谓嵌套类：即指一个类可以嵌套在其他类中。

例如：
```kotlin
class Other{           // 外部类
    val numOuther = 1

    class Nested {      // 嵌套类
        fun init(){
            println("执行了init方法")
        }
    }
}

fun main(args: Array<String>) {
    Other.Nested().init()      // 调用格式为：外部类.嵌套类().嵌套类方法/属性
}

```
**注意**
> - 调用嵌套类的属性或者方法格式为： 外部类.嵌套类().嵌套类方法/属性。在调用的时候嵌套类是需要实例化的。
> - 嵌套类不能使用外部类的属性和成员

### 2.内部类
> 声明一个内部类使用inner关键字。 声明格式：inner class 类名(参数){}

```kotlin
class Other{            // 外部类
    val numOther = 1

    inner class InnerClass{     // 嵌套内部类
        val name = "InnerClass"
        fun init(){
            println("我是内部类")
        }
    }
}

fun main(args: Array<String>) {
   Other().InnerClass().init()  // 调用格式为：外部类().内部类().内部类方法/属性
}
```
***注意**
- 调用内部类的属性或方法的格式为：外部类().内部类().内部类方法/属性。在调用的时候嵌套类是需要实例化的。
- 内部类不能使用外部类的属性和成员

#### 匿名内部类
> 作为一名Android开发者，对匿名内部类都不陌生，因为在开发中，匿名内部类随处可见。比如说Button的OnClickListener，ListView的单击、长按事件等都用到了匿名内部类。 一般的使用方式为定义一个接口，在接口中定义一个方法。

```kotlin
class Other{
    
    lateinit private var listener : OnClickListener

    fun setOnClickListener(listener: OnClickListener){
        this.listener = listener
    }
    
    fun testListener(){
        listener.onItemClick("我是匿名内部类的测试方法")
    }
}    

interface OnClickListener{
    fun onItemClick(str : String)
}

fun main(args: Array<String>){
    // 测试匿名内部类
    val other = Other()
    other.setOnClickListener(object : OnClickListener{
        override fun onItemClick(str: String) {
            // todo
            println(str)
        }
    })
    other.testListener()
}
```

### 内部类和继承的区别
- 从声明类上看，继承的两个类单独声明，子类通过（子类：父类）继承父类，而内部类必须声明在外部类里面，并且用关键字inner标记
- 从访问上看，继承的父类不能直接访问子类，外部类可以通过“外部类().内部类()”访问内部类，继承的子类能直接访问父类公开的成员属性及方法，而内部类值能通过this@外部类的方式访问外部类的属性和方法
- 从能否覆盖上看，继承的子类能覆盖父类用open标记的属性和方法，内部类不能覆盖外部类的属性和方法
- 从定义对象的方法看，继承的子类定义为val/var son = Son()，内部类的定义为 val/var inter = Outer().Inter()

