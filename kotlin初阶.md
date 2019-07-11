## 类的写法
```python
//user继承Person类
class User:Person(){
    
    //val 表示可读类型（相当于Java中final）
   private val str:String = "user"  
    
    //var 表示可读可写类型
    private var str:String = "hello"
    
    var i:Int = 100
    
    //如果右边的值能推断出左边的类型，类型可以不用写（：Int）
    var i2 = 200
    
    //?表示这个值可以为空
    var et1:EditText? = null
    
    //lateinit表示等会初始化的值，不需要初始值
    lateinit var et2:EditText
    
    //fun 方法的声明（默认是子类不可重写的，如果希望子类可以重写加open关键字）
    //(i:Int) 表示Int类型的参数
    //:Int 表示返回类型
    fun test(i:Int,i2:Double):Int{
        // !! 代码运行，如果et1为空报空指针
        et1!!.setText("hello");
        // ? 代码运行，先判断et1是否为空，为空不调用函数
        et1?.setText("hello");
        
    }
    
}
```
## 继承和实现
==在kotlin中继承和实现都是：，需要使用父类构造器的需要加（）==
```python
继承
Java
        class User extend Person{}
     
Kotlin
        class User:Person(){}

实现
Java
        class User implements Person{}
       
Kotlin
        class User:Person{}

```

## 构造器
==有主构造器，想再申明一个构造器，需要在构造器中调用主构造器。==
```python
//无继承关系的类
class User {

    var name:String? = null
    var code:String? = null
    //空参构造
    constructor(){
        
    }
    //有参构造
    constructor(name:String?,code:String?){
        this.name = name
        this.code = code
    }
}
//或者写入主构造器
class User (name:String?,code:String?){

    var name:String? = null
    var code:String? = null
    
    //init函数获取主构造器函数赋值（init函数和成员变量一起执行，所以init函数中需要的值需要提前初始化好）
    init{
        this.name = name
        this.code = code
    }
}

//有继承关系的类
class Teacher2: User {

    constructor(name:String?,code:String?):super(name,code){

    }
}

class Student(name: String?, code: String?) : User(name, code) {

}

```

## 类的调用
```python
    var user:User = User();
```

## 关键字lateinit

==lateinit表示等会初始化的值，不需要初始值==

```python
    lateinit var et1:EditText
```
## 关键字as

==as表示强转==

```python
Java
        TextView tv = (TextView) v;
       
        
Kotlin
        var tv:TextView? = v as TextView

```

## 关键字is

==is表示是否属于（相对于Java中instanceof）判断完成后就可以直接使用方法，不需要强转动作==

```python
    if(v is TextView){
       v.setText("hello"); 
    }
```

## 关键字open
open 修饰类和方法的，一般类和方法默认是final的，不可继承，不可重写，open修饰后表示可继承可重写。

## 关键字const
const 静态常量

## 关键字internal
==internal 修饰类时表示同一模块下被访问，不同包不可以访问==

## 数组
```python
    val array = arrayOf("a", "b", "c")
    
    //遍历数组取下标
    for (index in array.indices){
            println("index=$index")//输出0，1，2
        }

    //遍历数组取对象
    for (element in array){
            println("element=$element")//输出a,b,c
        }
        
    //遍历数组取下标和对象  
    for ((index,e) in array.withIndex()){
            println("下标=$index----元素=$e")
        }

```

## when判断(类似于Java中switch)
```python
    val x1 = 1;
    var x2 = 0;
    //1和2 x2 = 2
    when(x1){
        1,2 -> x2 = 2
        3 -> x2 = 3
    }
    println(x2) //2
    
    //in 表示区间
    when(x1){
        in 1..10 -> x2 = 2
        else ->{
            x2 = 3
        }
    }
    println(x2)//2
    
```

## 循环
```python
//repat函数从0开始的循环100次    
repeat(100){
        println(it)//输出0123..99
    }
    
    
    //循环1-100（包括1和100）
    for (index in 1..100){
            print(index)
        }

    //downTo倒序100 downTo 1 表示100到1倒序
    for (index in 100 downTo 1){
            print(index)
        }
        
    //step间隔    
     for (index in 1..100 step 2){
            print(index)//会输出1..3..5......
        }
    
    //until 包含末尾元素的区间
    for (index in 1 until 10){
        println(index)//输出1..9
    }        

```

## 静态函数

1、直接在kt文件中声明函数，不放在类中，调用时倒入文件，直接调用函数，不需要写类名；

```python
import com.shinhoandroid.test0708.User

class Person{
    
    open fun test(){
        //text()定义在User文件中
        text();
    }

}
//如果java中需要调用，需要给文件取一个JvmName
@file:JvmName("User")
package com.shinhoandroid.test0708
fun text(){

}

//java中调用就可以用User.text()来调用

```
2、通过关键字object申明
```python
    object Utils{
    fun func(){
    }
}

//调用
class Person{
    open fun test(){
        Utils.func()
    }

}

```
3、在类中定义companion object
```python
class Utils{
    companion object{
        public lateinit var app: Application
        fun funb(){
        }
    }
}

//调用
class Person{
    open fun test(){
        Utils.app
        Utils.funb()
    }
}

```
4、在Java中调用kt代码中静态方法通过注解@JvmStatic或者调用Companion函数在调用方法
```python
//@JvmStatic注解
class Utils{
    companion object{
        @JvmStatic
        public lateinit var app: Application
        @JvmStatic
        fun funb(){
        }
    }
}

//Java中调用
public class Dog {
    public void func(){
        Application a = Utils.app;
        Utils.funb();
    }
}

//Companion函数方式调用
class Utils{
    companion object{
        @JvmStatic
        public lateinit var app: Application
        fun funb(){
        }
    }
}

//Java中调用
public class Dog {
    public void func(){
        Application a = Utils.app;
        Utils.Companion.funb();
    }
}

```

## 匿名内部类
使用object关键字

```python
et.setOnClickListener(object: View.OnClickListener{
            override fun onClick(v: View?) {
                //调用外部类MainActivity中et2
                this@MainActivity.et2?.setText("lpf") 
            }
        })
```
## 枚举
```python
    enum class State{
    //三个对象
    PLAYBACK {
        override fun stateName() :String{
            return "有回放";
        }
    },
    LIVE {
        override fun stateName() :String{
            return "正在直播";
        }
    },
    WAIT {
        override fun stateName() :String{
            return "等待中";
        }
    };
    //定义方法
    abstract fun stateName():String;
}
```

## class获取

```python
//java写法
Class c = person.getClass(); //对象获取
Class cc =Person.class;//类获取


//kotlin写法
//在kotlin中有两种class，一种javaClass，一种kClass
//对象获取
person.javaClass// javaClass
person::class.java // javaClass
//类获取
Person::class// kClass
person.javaClass.kotlin// kClass
(Person::class as Any).javaClass// javaClass
Person::class.java // javaClass



```