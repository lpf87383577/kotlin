## 构造器优化
```python
class User (var name:String?,var code:String?){
    
    //在变量前加上var或者val，会直接生成成员变量，不需要使用init函数
    
}
```
## 关键字data
data 修饰类，用于生成类的equals()/hashCode()/toString()/componentN()/copy()方法

```python

data class User (var name:String?,var code:String?){

}

```
## 操作符 ?：
表示如果为null，就用：后面的值，不为null就用自身的值。

```python
    var user = User("dad","daw")
    println(user.name?:"user")
    user.name = null
    println(user.name?:"user")
    
    结果：
    dad
    user
    
```

## when函数有返回值
==如果when里面的操作都是对同一个对象操作，那可以直接在when函数接受值。==
```python
var user = User("dad","daw",8)
    val v = when(user.age){
        1,2,3,4,5,6->"婴幼儿"
        7,8,9,10,11->"小学生"
        else->"中学生"
    }
    println(v)

    结果：
    小学生
```

## 集合遍历
```python
//一般写法
 var list = ArrayList<String>()
    list.add("a")
    list.add("b")
    list.add("c")
    
    var list2 = ArrayList<String>()
    var list3 = ArrayList<String>()
    
    for(l in list){

        if (l=="a"){
            list2.add(l)
        }
        if (l=="b"){
            list3.add(l)
        }
    }
    
//forEach
list.forEach{
        if (it=="a"){
            list2.add(it)
        }
        if (it=="b"){
            list3.add(it)
        }
    }

//如果内部判断只有一种
var list3 = list.filter {
        it=="a"
    }
    
```
## 函数嵌套
==函数可以嵌套，内部函数可以获取外部函数的参数==
```python

fun funb(){

    fun func():Int{
        return 1+1
    }

    println(func())
}
```
==针对函数只有一行代码的时候可以用=连接==

```python
//fun func():Int{
//    return 2+2;
//}
//可以直接写成
fun func() = 2+2

//没有return也用 = 连接
//fun funb():Int{
//    2+2;
//}
//可以直接写成
fun funb() = 2+2

```
## 函数默认参数
==方法重载时可以简化代码==

```python

fun func(i:Int){
   func(i,"") 
} 

fun func(i:Int,s:String){
    
}
//可以简化为
@JvmOverloads  //java调用单参数不会报错
fun func(i:Int,s:String = "lpf"){
    //这样可以调用方法时可以只传一个参数（和Python一样）
}

```
## 单参数函数简化
可以为其他函数添加拓展函数，如果拓展函数和成员函数冲突，会调用成员函数。

```python

//fun funb(i:Int):Int{
//    return i+1
//}

//函数只有一个Int参数，在方法加上Int.,类似于给Int添加了一个funb函数，调用时可以用Int调用
//fun Int.funb():Int{
//    return this+1
//}

//函数只有一行，改成=
fun Int.funb() = this+1

//调用，直接那Int类型数调用即可
2.funb()

//也可以为类添加拓展变量
//为ViewGroup添加firstchild变量
val ViewGroup.firstchild: View
        get() = getChildAt(0)

//调用
vg.firstchild

```

## 内联函数
inline修饰的函数，调用时少一次调用栈，类似于代码copy进去了
```python

/*
* 正常调用是
* main->进栈
* fund->进栈
* i1+i2
* fund->出栈
* main->出栈
* 
* inline修饰后调用
*  main->进栈
* i1+i2
* main->出栈
* */
inline fun fund(i1:Int,i2:Int):Int{
    return i1+i2
}


fun main(){
    fund(1,2)
    }

//转成Java代码
public static final void main() {
      byte i1$iv = 1;
      int i2$iv = 2;
      int var10000 = i1$iv + i2$iv;
   }
    
```

## lambda
kotlin的lambda相对于java8的lambda语法更简洁，事实上它们做的事情是一样的，都是替代了匿名内部类对象。（长的有点像闭包，但是本质不一样）
```python

//基本写法action函数只能定义一个参数
fun funa(i:Int,action:()->Unit){
    println(">>>>>")
    action()
    println("<<<<")

}
//如果只有一个参数
fun funb(action:()->Unit){
    println(">>>>>")
    action()
    println("<<<<")

}

fun main(){
    //调用funa函数，如果lambda是最后一个参数，{}可以写在()外面,如果只有一个lambda参数，()可以不写
//    funa(1){
//        println("hello")
//    }

    funb{
        println("hello")
    }
}

```
## 委托by
在委托模式中，有两个对象参与处理同一个请求，接受请求的对象将请求委托给另一个对象来处理。Kotlin 直接支持委托模式，更加优雅，简洁。Kotlin 通过关键字 by 实现委托。

==委托类==
```python

interface Person{
   fun eat()
}

class Student :Person{
    override fun eat() {
        println("学生吃饭")
    }
}

//java委托
public class Teacher implements Person {

    Person p;

    public Teacher(Person p){
        this.p = p;
    }

    @Override
    public void eat() {
        p.eat();
    }
}

//kotlin委托
//Teacher2 继承Person的所有方法，但是实现有传进来的Person决定
class Teacher2(p:Person) :Person by p{
    
}

//调用
Teacher2(Student()).eat()
 
//结果
学生吃饭

```
==委托属性==

```python
//委托给类
class Example {
//p的值委托给Delegate类
    var p: String by Delegate()
}


class Delegate{
    operator fun getValue(example: Example, property: KProperty<*>): String {
        return "P值被获取了"
    }

    operator fun setValue(example: Example, property: KProperty<*>, s: String) {
        println("P被赋值${s}了")
    }
}

//执行
fun main(){

    println(Example().p);

    Example().p = "lpf";

}

//结果
P值被获取了
P被赋值lpf了

//委托给方法（适用于一些默认值）
//因为只执行一次，所以只能用val，不用var，加上lazy表示只在用到str的时候才会赋值

class Example {
    val str: String by lazy {
        println("computed!")     // 第一次调用输出，第二次调用不执行
        "Hello"
    }
}

//执行
fun main(){

    var e =  Example();
    println("Example被初始化了")
    e.str.toString();

}


//结果
Example被初始化了
computed!

```
## 标准函数（作用域函数）
let、with、run、apply、also函数，几个函数功能差不多。

```python
//在函数体内使用it替代object对象去访问其公有的属性和方法
//返回值为函数块的最后一行或指定return表达式。
object.let{
   it.todo()
}


//它是将某对象作为函数的参数，在函数块内可以通过 this 指代该对象，也可以不用。
//返回值为函数块的最后一行或指定return表达式。
with(object){
   //todo
   
}

//返回值为函数块的最后一行或指定return表达式。
//优点可以判空
object.run{
//todo
}

//apply函数的返回的是传入对象的本身。
object.apply{
//todo
}

//和let类似，不同的是返回的是传入对象的本身
object.also{
//todo
}


```
函数名 | 函数体内使用的对象 | 返回值	| 是否是扩展函数 | 	适用的场景
---|---|---|---|---
let|it|闭包形式返回	|是	|适用于处理不为null的操作场景
with|this指代当前对象或者省略|闭包形式返回	|否|	适用于调用同一个类的多个方法时，可以省去类名重复，直接调用类的方法即可
run	|this指代当前对象或者省略|	闭包形式返回|是|	适用于let,with函数任何场景。
apply|this指代当前对象或者省略|	返回this|是|一般用于初始化一个对象实例的时候，操作对象属性，并最终返回这个对象。
also|it指代当前对象	|返回this|	是|	适用于let函数的任何场景，一般可用于多个扩展函数链式调用
```python


```