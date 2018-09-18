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

