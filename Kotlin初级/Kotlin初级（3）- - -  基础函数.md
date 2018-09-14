## 函数的声明和使用

> Kotlin中函数的声明使用关键字 **fun**
> 格式为：可见性修饰符 fun 函数名(参数名 ：类型)：返回值{}

例如：

```kotlin
fun main(){
    
}
```

- 上面的例子没有==可见性修饰符==，因为Kotlin中默认为public
- 函数没有返回值时可以省略

## 函数的作用范围

### 1.成员函数

> 成员函数是指在类或对象中的内部函数
例如：
```kotlin
class Demo{
    fun main(){}
}
```

### 2.本地函数
> 本地函数允许把小函数声明在其他函数内部，甚至可以多层嵌套

例如
```kotlin
fun printArea(w: Int,h: Int){
    fun area(w: Int,h: Int){
        return w * h
    }
    val area = area(w,h)
    print(area)
}
```
area函数在printArea外部无效，它只服务于printArea。这在实现一个大函数时隐藏实现的细节是非常有用的。除此之外，本地函数还有一个好处就是可以访问嵌套住函数中的变量，例如：

```kotlin
fun printArea(w: Int,h: Int){
    fun area() = w * h
    
    val area = area(w,h)
    print(area) 
}
```
### 3.顶层函数
> 所谓顶层函数，即这些函数不属于任何源码文件的小集团（class，对象，interface），而是直接定义在源码文件中的。他们在所有小集团的层级之上。
> 在定义通用性的工具栏和帮助类函数时非常有用，源码的各个部分可能都需要用它。

## 命名参数和默认参数

### 1.命名参数
> 可以在调用函数时把参数的名字写出来。好处是一旦参数众多，调用时会看的比较清楚，代码可读性高。

例如：
```kotlin
fun printPerson(p: Person){
    print(p.match(age = 20,
    name = "hankkin",
    nickname = "xxx",
    sex = "男",
    weight = "120"))
}
```
当然在AS中，默认会显示参数的hint，很方便了，所以一般很少用。

### 2.默认参数
> 在Kotlin中可以定义一个或者多个默认参数，在被调用时如果不指定，则使用默认值。

```kotlin
fun valueOf(unscaledVal: Int = 0,scale: Int,prec: Int = 1)
```

## 函数操作符

> 函数操作符用了一个符号来表示。Kotlin中的函数有很多内置的操作符。例如array[1],[1]就相当于数组的.get(1)方法

### 1.操作符重载
> Kotlin允许为预定义操作符提供自定义的实现。这些操作符具有固定符号表示(如+ - * /),固定的优先级precedence。有相应的成员函数member function或扩展函数extension function，重载操作符的函数必需要用operator修饰符标记

### 2.基础操作符

|      操作|函数名      |
| ---- | ---- |
| !x | x.not() |
| -y | y.unaryMinus() |
| +z | z.unaryPlus() |
| a..b | a..rangeTo(b) |
| c + d | c.plus(d) |
| e - f | e.minus(f) |
| g * h | g.times(h) |
| m % n | m.mod(n) |
|    i/j  | i.div(j) |

## 函数扩展

### 1.扩展函数的优先级
扩展函数不能重载类或者接口中已经定义的函数。如果你定义了一个与既有函数一摸一样的扩展函数，名字一样，参数一样，这个扩展时无效的。

### 2.扩展函数的作用范围
通常我们用顶层函数做扩展，但也可以在类中做扩展

### 3.扩展函数在子类中的重载

子类中可以重载成员扩展函数，前提是这个类是open，即可重载的。在这种情况下，子类的函数接受者类型是由运行时的实例决定的，而扩展的接受者类型始终时编译时就确定的，也就是静态的

### 4.infix中缀函数
中缀函数跟赋值操作符有点像，不同的是名称可以是任意的。例如Kotlin自带的to函数，可以把两个变量凑成一个二元祖。Kotlin中可以把成员函数定义成中缀。因为中缀函数是二元的，必须有2个参数，第一个很显然是实例，第二个是函数的参数




