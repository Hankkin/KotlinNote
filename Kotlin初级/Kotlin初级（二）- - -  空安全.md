## 空类型、空安全

### 变量的定义
Kotlin中的变量修饰符只有两个 
- val修饰的变量不能重新被赋值
- var修饰的变量可以被重新赋值

> var修饰的变量不可为null，val修饰的可为null


```kotlin
val a: Int? = null
var b: Int
```

> 变量a在使用的时候需要判断改变量是否为null，变量b则不需要了，因为这个变量永远不会为null

### 可空类型的判断

#### if else


```kotlin
val str: String? = null
if(str == null){
    
}else {
    
}
```

#### 使用?.判断
如果可空变量为null时，返回null
这种用法大量用于链式操作，能有效避免空指针异常


```kotlin
var str: String? = null
print(str?.length)
```
### 链式调用


```kotlin
print(str?.length?.plus(5)?.minus(10))
```

#### 函数中使用可空类型
当一个函数有返回值时，如果返回的值为可空类型，那么方法的返回值类型后面也要加？
### 操作符
####  let
- let操作符的作用：当使用符号?.验证的时候忽略掉null
- let的用法： 变量?.let{}


```kotlin
val arrTest : Array<Int?> = arrayOf(1,2,null,3,null,5,6,null)

// 传统写法
for (index in arrTest) {
    if (index == null){
        continue
    }
    println("index => $index")
}

// let写法
for (index in arrTest) {
    index?.let { println("index => $it") }
}
```

#### ?:操作符

> 当我们定义了一个可空类型的变量时，如果该变量不为空，则使用，繁殖使用另外一个不为空的值


```kotlin
val testStr : String? = null

var length = 0

// 例： 当testStr不为空时，输出其长度，反之输出-1

// 传统写法
length = if (testStr != null) testStr.length else -1

// ?: 写法
length = testStr?.length ?: -1

println(length)
```

#### Evils操作符
> Evils其实不是一个操作符，而是evil的复数，而evil的意思在这里可以理解为屏蔽、安全的操作符，这样的操作符有三种：
> -  ?:  这个操作符表示在判断一个可空类型时，会返回一个我们自己设定好的默认值
> -  !! 这个操作符在判断一个可空类型时，会抛出空指针异常
> - as? 这个操作符表示为安全的类型转换 

#### !! 操作符
> !! 操作符可谓是给爱好空引用异常的开发者使用，因为在使用一个可空类型变量时，在改后面加上！！操作符，会显示抛出的空指针异常

#### as?操作符
> 其实这里是指as操作符，表示类型转换，如果不能正常转换的情况下使用as?操作符。当使用as操作符的使用不能正常的转换的情况下会抛出类型转换（ClassCastException）异常，而使用**as?**操作符则会返回null,但是不会抛出异常

## 总结

- 项目中会抛出空指针**NullPointerException**的情况
	- 在可空类型变量的使用时，用了!! 操作符
	- 显示抛出空指针 throw NullPointerException()
	- 外部Java代码导致的
	- 对于初始化，有一些数据不一致（如一个未初始化的this用于构造函数的某个地方）
- 项目中会抛出类型转换**ClassCastException**的情况
	- 在类型转换中使用了**as**操作符
	- 使用了**toString()**、toInt()等方法不能转换
	- 外部Java代码导致
- 尽量避免使用的操作符
	- 尽可能的不要使用!!操作副，多使用**?:**、?.操作符，以及**let{}**函数
	- 尽可能的使用**as?**操作副去替换掉**as**
	- 在不确定是否可以安全转换的情况下不使用**toString()**等方法

