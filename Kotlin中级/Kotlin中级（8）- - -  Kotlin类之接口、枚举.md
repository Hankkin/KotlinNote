## 一、接口
> 在Kotlin中，一个类只能继承一个普通类或者抽象类。通过接口我们可以进行多实现

**interface**

```kotlin
interface Demo{
    
}
```
- 关键字：冒号(:)，这一点是和Java不同的。Java中使用接口使用的是implements关键字
- 在Kotlin中冒号(:)使用的地方很多：
1.用于变量的定义
2.用于继承
3.用于接口
4.方法的返回类型声明



**接口冲突**

例如：

```kotlin
interface Apple{
    fun printSelf()
    fun give() = print('')
}

interface Banana{
    fun printSelf() = println("")
    fun give = print("")
}

class Person : Apple,Banana{
    override fun printSelf() {
        
    }

    override fun give() {
        super<Apple>.give()
        super<Banana>.give()
    }
    

}
```
> 在这里，大家应该看到了Apple和Banana这两个接口都声明了give方法，实现了这两个接口的Person类，在实现give方法时使用super<接口或超类的名称>.方法


## 二、枚举类


