> Lambda表达式：本质上就是可以传递给其他函数的一小段代码

## 一、Lambda表达式和成员引用

### 1.Lambda简介：作为函数参数的代码块

例如：当一个事件发生的时候运行这个事件处理器 或者 把这个操作应用到这个数据结构上所有的元素。我们在Java中可以使用匿名内部类来实现，而函数式编程中可以把函数当作值来对待。

例如一个按钮的点击事件 在Java中我们可以这样写：
```
btn.setOnClickListener(new OnclickListener(){
    void onClick()
})
```
用lambda我们可以这样写：


```
btn.setOnClickListener{}
```

### 2. lambda和集合

找到一个类中某个属性最大值的对象


```
val data = listOf(Person("name",23),Person("name1",22))
print(data.maxBy{it.age})
```

### 3. lambda表达式的语法

一个lambda把以小段行为进行编码，你能把它当作值到处传递。它可以被独立的声明并存储到一个变量中

```
{x: Int,y: Int -> x+y}
```
Kotlin中的lambda表达式始终用花括号包围。注意实参并没有用括号括起来。箭头把实参列表和lambda的函数体隔开

### 4. 在作用域中访问变量

当在函数内声明一个匿名内部类的时候，能够在这个匿名类内部引用这个函数的参数和局部变量。也可以用lambda做同样的事情。如果函数内部使用lambda，也可以访问这个函数的参数，还有lambda之前定义的局部变量。


```
fun a(data: Collection<String>,x: String) {
    data.forEach{
        print("$x $it")
    }
}
```
这里Kotlin和Java的一个显著区别就是，在Kotlin中不会仅限于访问final变量，在lambda内部也可以修改这些变量。和Java不一样，Kotlin允许在lambda内部访问非final变量甚至修改它们。从lambda内访问外部变量，我们称这些变量为*lambda捕捉*。注意：默认情况下，局部变量的生命期被限制在声明这个变量的操作中。但是如果它被lambda捕捉了，使用这个变量的代码可以被存储并稍后再执行

### 5. 成员引用


```
val age = Person::age
```
这种表达式被成为*成员引用*,它提供了简明语法，来创建一个调用单个方法或者访问单个属性的函数值







## 二、以函数式风格使用集合

## 三、序列：惰性的执行集合操作

## 四、在Kotlin中使用Java函数式接口

## 五、使用带接收者的lambda