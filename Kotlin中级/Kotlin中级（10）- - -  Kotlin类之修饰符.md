## open和final
```kotlin
open class Person{
    open var name: String = ""
    var age: Int = 0
    var height: Double = 0.0
    var weight: Double = 0.0
}
class Student : Person(){
    override var name: String
        get() = super.name
        set(value) {}
}
```
我们可以看到Person用open修饰了，证明它可以被继承，如果我们不用open关键字修饰，通过java字节码我们可以看出，原来的代码转化成了Java之后被默认加上了**final**，意味着不可被继承

```kotlin
 class Person{
    open var name: String = ""
    var age: Int = 0
    var height: Double = 0.0
    var weight: Double = 0.0
}
```
```java
// ================Person.class =================
// class version 50.0 (50)
// access flags 0x31
public final class Person {


  // access flags 0x2
  private Ljava/lang/String; name
  @Lorg/jetbrains/annotations/NotNull;() // invisible
   .....
```

### 多继承
```kotlin
 open class Person{
    open var name: String = ""
    var age: Int = 0
    var height: Double = 0.0
    var weight: Double = 0.0
}
 
 open class Student : Person(){
     override var name: String = "xxx"
 }
 
 class GoodStudent : Student(){
     override var name: String
         get() = super.name
         set(value) {}
 }
```

在Kotlin中，我们在继承一个类后覆盖进行了override修饰，这个修饰也使这个属性默认open化

### 接口
> 在Kotlin中，接口本身和它内部的方法和属性都是默认加上open修饰符的，和普通类默认加上final修饰符石不同的