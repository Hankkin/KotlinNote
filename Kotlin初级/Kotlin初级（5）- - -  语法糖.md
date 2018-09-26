## when

```kotlin
when(x){
    1 -> print("1")
    2 -> print("2")
    else -> {
        print("x is neither 1 nor 2")
    }
}
```
when会对所有的分支进行检查直到有一个条件满足。when可以用作表达式或声明。如果用做表达式的话，那么满足条件的分支就是总表达式。如果用做声明，那么分支的值就会被忽略。（像if表达式一样，每个分支是一个语句块，而且它的值就是最后一个表达式的值）

在其他分支都不匹配的时候默认匹配else分支。如果把when作为表达式的话else分支是强制的。除非编译器可以提供所有覆盖所有可能的分支条件。

例如：
> 当when语句作为一个表达式的时候，这时候如果没有覆盖所有分支情况，编译器会报错**when expression must be exhaustive,add necessary 'else'branch**
![屏幕快照 2018-09-26 上午10.28.50](/Users/huanghaijie/Desktop/屏幕快照 2018-09-26 上午10.28.50.png)

```kotlin
val index = 0
    val result = when(index){
        0 -> println("$index")
        1 -> println("$index")
        2 -> println("$index")
        3 -> println("$index")
        else -> {
        }
    }
```
但是当你把变量声明去掉，让when只作为一个表达式的时候else就可有可无了
```kotlin
when(index){
        0 -> println("$index")
        1 -> println("$index")
        2 -> println("$index")
        3 -> println("$index")
    }
```

## for循环
Kotlin中没有像Java中常规的for循环，for(int i=0;i<5;i++),这样的在Kotlin中使用了区间的概念，区间的本质就是两个值之间的间隔，这两个值通常是数字：一个起始值，一个结束值，用..运算符表示区间
这里的区间是一个**闭区间**

```kotlin
for (i in 0..100){
        println(i)
    }
```
### downTo
同样你也可以使用关键字**downTo**来实现降序循环
```kotlin
for (i in 100 downTo 1){
        println(i)
    }
```
### reversed

也可以使用reversed()来将正序的反转
```kotlin
for (i in (100 downTo 1).reversed()){
        println(i)
    }
```
当然这里没有什么意义，只是顺便介绍了**reversed()**

### step

可以使用step控制步长
```kotlin
for (i in 1..100 step 2){
        println(i)
    }
```

### until

可以使用until, 表示包含左边, 不包含右边的范围, 数学符号为[start, end)
是一个左闭右开的区间
```kotlin
for (i in 1 until 100 ){
        println(i)
    }
```

Java中遍历某个数组的时候可以通过(item : List)形式来遍历，同样Kotlin中可以使用 (item in list)来实现
```kotlin
val list = mutableListOf(1,2,3,4,5)
    for (i in list){
        println(i)
    }
```

你也可以通过list.indices来遍历数组的索引，indices是一个索引的集合

```kotlin
val list = mutableListOf(1,2,3,4,5)
    for (i in list.indices){
        println(i)
    }
```

由于业务需求千变万化，所产生的遍历条件也千变万化，所以Kotlin同样保留了while和do while，和Java中基本一致，不做详细介绍

### for each
kotlin中还为list集合提供了特殊的遍历方式***for each*

```kotlin
val list = mutableListOf(1,2,3,4)
    list.forEach {
        println(it)
    }
```
可以看一下Kotlin的源码，也就是调用了for(i in list)

```kotlin
/**
 * Performs the given [action] on each element.
 */
@kotlin.internal.HidesMembers
public inline fun <T> Iterable<T>.forEach(action: (T) -> Unit): Unit {
    for (element in this) action(element)
}
```
这里涉及到了kotlin中的高阶函数，详情在高阶函数中

### forEachIndexed
同样Kotlin中还支持了forEachIndexed，和forEach不同的是，前者比后者多了两个参数index和i，显而易见

```kotlin
val list = mutableListOf(1,2,3,4)
    list.forEachIndexed { index, i ->
        println("索引值：$index,值为：$i")
    }
```


## 遍历map

