# java基础

### 一，数据类型

#### 基本类型

* byte /8位1字节

* char /16位2字节

* short /16为2字节

* int /32位4字节(根据操作系统不确定性)

* float /32位4字节

* double /64位8字节

* boolean /true/false

  **jvm会在编译时期将boolean类型的数据转换为int类型，用1来表示true，0表示false；同时jvm支持boolean[]类型数组，但是是通过读写byte[]数组来实现的**  

#### 包装类型

* 八大基本类型都有属于自己的包装类型，基本类型与其包装类之间的复制使用装箱和拆箱完成，java中有对八大基本类型进行自动装箱和自动拆箱

  ~~~java
  Integer x = 4; //装箱，调用Integer.valueOf(4)缓存池技术
  int y = x ;    //拆箱，调用了X.intValue()方法拿到包装类的中的值
  ~~~

  

#### 缓存池技术

* 基本类型在java的底层中也是以类的形式存在的，但是过多的类的创建会消耗大量的资源，所以在遇到范围内设定了缓存池，用于减少多次创建包装类的消耗

~~~java
new Integere(123); //每次new一个对象都是重新创建一个新的对象，不会在已有的池中去寻找合适的引用
Integer.valueOf(123);  //默认的使用方法，在自动装箱时会调用，调用缓存池中的对象，多次调用会获得同一个对象的引用
~~~

* 基本的缓存池：
* boolean  / true and false
* byte / all values
* short  / -128 ~ 127
* int  /-128 ~ 127
* char  / \u0000 ~ \u007f

**另外Integer的缓存池可以在开启jvm时进行调优，通过 -XX：AutoBoxCacheMax=<size> 来指定其Integer缓存池的最大上限，之后在jvm启动后Integer在创建时就会使用这个给定的值。**



### 二，String

#### 概览

* String被声明为final，表示其不可被继承(包装类型也不可被继承)
* 其内部使用char数组存储数据(java8) ，java 9之后为了细粒度使用byte[]数组，同时增加了coder字段用于标识编码
* 用于存储的value数组被声明为final，表明其不能被外部直接调用，保证了String的不可变性

#### String不可变的好处

1. 可以缓存hash值，因为String经常被用于Map的key，不可变的特性使其hash值也不会变，减少计算
2. 为字符串池的实现提供的支持
3. 保证了参数不可变的安全性
4. 线程安全，因为其在多个线程同时调用时也是不可改变的

#### String，StringBuffer，StringBuilder

1. 可变性
   * String不可变
   * StirngBuffer和StringBuilder可变
2. 线程安全
   * String不可变，所以线程安全
   * StringBuilder不是线程安全
   * StringBuffer是线程安全的，内部所有的属性和方法都使用了synchronized锁进行了修饰
3. 字符串在创建时直接赋值，StringBuilder等使用append("")方法向其中添加新的字符字段

#### String 字符串常量池

* 在java7 之前，常量池在运行时常量池，也即永久代。在java7之后，取消了永久代，常量池就移到了队中，交给了GC进行管理？

* 用于保存所有字符串对象(字符串字面量)，这些对象在编译时期就确定了，也可以通过String的intern()方法在运行过程中将字符串添加到常量池中
* 在创建一个字符串池，都会调用intern()方法，若字符串中已经有了这个字符串(使用equals()进行比较)，则会直接返回这个已有的字符串的引用；负责会在其中再创建这个对象，并返回引用

~~~java
String s = ""; //会在常量池中查找是否有相等的对象，
String s = new String(""); // 会直接在堆中new一个新的对象，再在常量池中创建一个对象，会创建两个对象(常量池中无这个字符串的话)
~~~

### 三，运算

#### 参数传递

* java中所有的参数传递都是以值传递的形式，而不是引用传递
* 对象作为参数时本质上是将其所在的地址作为参数进行传递

#### float和double

* java中不能够隐性的执行向下转型，会使得数值的精度降低

~~~java
// float f = 1.2;  错误写法，小数类型的字面量为double，不能进行向下转型

float f = 1.1f; //正确写法，
~~~

#### 隐式类型转换

* short的类型精度比int低，所以若直接进行赋值是需要进行强制转型的

~~~ java
short s = (short) 1; //强制转换

short s += 1;  
s++;    //有效写法，其进行了强制转换

//等价于
s = (short)(s + 1);
~~~

### 四，关键字

#### final

1. 数据类型
   * 对基本类型进行修饰，可以保证这个基本类型的数值不会再方式改变
   * 对引用类型进行修饰，时该引用不能够改变，即不能够再引用其他对象，但是可以改变被引用对象自身。可以看做其将引用类型所引用的地址进行了不可变的操作。
2. 方法
   * 声明方法不能够被子类重写
   * private方法被隐式的指定为final，即不能够其子类所得到，如果子类自己声明了一个和其父类private方法一致的方法，则这个子类中的方法为一个全新的方法。

#### static

* 静态语句属于类，而不属于该类的实例化对象。
* 实例变量(非静态变量)：每次创建一个实例化对象都会生成一份属于自己的实例变量，其和该实例是同存亡的
* 初始化顺序：
  1. 优先静态语句或代码块(取决于位置先后)，在类加载时就存在了
  2. 若进行了类的实例化，则再加载其中的非静态变量(实例变量)，
  3. 最后再加载其中的构造函数并完成初始化
* 若有继承关系，则优先级如下
  * 父类（静态变量、静态语句块）
  * 子类（静态变量、静态语句块）
  * 父类（实例变量、普通语句块）
  * 父类（构造函数）
  * 子类（实例变量、普通语句块）
  * 子类（构造函数）

### 五，Object类的通用方法

#### 概览

* equals()方法 其是属于Object类，所以其余所有类都可以调用这个方法，用于比较数值是否相等
* hashCode()，若hash集合想要将类对象加入到其中，则一般需要这个类实现hash方法，以使其计算hash值时能够得到正确的值
* toString()，一般需要打印对象则会对该方法进行重写，不然打印的是属于该类的地址
* clone()，其在Object中是一个protected方法，所以如果一个类不对该方法进行重写，则其余类不过使用该方法克隆这个对象。一般不推荐使用。
* 浅拷贝：拷贝其基本类型的值和其对象的引用
* 深拷贝：拷贝其基本类型的值和一个新创建的对象

* 使用clone即复杂还有不小的风险，所以推荐使用拷贝构造函数(反射)或者拷贝工厂来拷贝一个对象



### 六，继承

#### 抽象类和接口

* 抽象类中的方法不全为抽象，但若一个类中有一个抽象方法，则这个类必须被声明为抽象类
* 抽象类不能被实例化，只能被继承
* 接口是抽象类的延续，在java8 之前接口可以看为一个完全抽象的类，也即其不能有任何得到实心方法
* java9之后接口也可以有默认的方法实现，同时可以将字段定义为私有的。以便更方便的使用封装
* 在很多情况下，接口优先于抽象类，因为其使用更加随意，而且消耗的资源更小



#### super

* 用于子类访问父类的构造方法，在子类默认初始化时，就会默认的访问父类的默认构造器
* 不过也可以在其中使用super(x, y)访问父类的其他构造器

* 如果子类重写了父类的方法，可以通过super.function();来访问父类的方法

### 七，重写与重载

**1. 重写（Override）**

存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法。

为了满足里式替换原则，重写有以下三个限制：

- 子类方法的访问权限必须大于等于父类方法；
- 子类方法的返回类型必须是父类方法返回类型或为其子类型。
- 子类方法抛出的异常类型必须是父类抛出异常类型或为其子类型。

使用 @Override 注解，可以让编译器帮忙检查是否满足上面的三个限制条件。

下面的示例中，SubClass 为 SuperClass 的子类，SubClass 重写了 SuperClass 的 func() 方法。其中：

- 子类方法访问权限为 public，大于父类的 protected。
- 子类的返回类型为 ArrayList<Integer>，是父类返回类型 List<Integer> 的子类。
- 子类抛出的异常类型为 Exception，是父类抛出异常 Throwable 的子类。
- 子类重写方法使用 @Override 注解，从而让编译器自动检查是否满足限制条件。

```java
class SuperClass {
    protected List<Integer> func() throws Throwable {
        return new ArrayList<>();
    }
}

class SubClass extends SuperClass {
    @Override
    public ArrayList<Integer> func() throws Exception {
        return new ArrayList<>();
    }
}
```

在调用一个方法时，先从本类中查找看是否有对应的方法，如果没有再到父类中查看，看是否从父类继承来。否则就要对参数进行转型，转成父类之后看是否有对应的方法。总的来说，方法调用的优先级为：

- this.func(this)
- super.func(this)
- this.func(super)
- super.func(super)

```java
class A {

    public void show(A obj) {
        System.out.println("A.show(A)");
    }

    public void show(C obj) {
        System.out.println("A.show(C)");
    }
}

class B extends A {

    @Override
    public void show(A obj) {
        System.out.println("B.show(A)");
    }
}

class C extends B {
}

class D extends C {
}
public static void main(String[] args) {

    A a = new A();
    B b = new B();
    C c = new C();
    D d = new D();

    // 在 A 中存在 show(A obj)，直接调用
    a.show(a); // A.show(A)
    // 在 A 中不存在 show(B obj)，将 B 转型成其父类 A
    a.show(b); // A.show(A)
    // 在 B 中存在从 A 继承来的 show(C obj)，直接调用
    b.show(c); // A.show(C)
    // 在 B 中不存在 show(D obj)，但是存在从 A 继承来的 show(C obj)，将 D 转型成其父类 C
    b.show(d); // A.show(C)

    // 引用的还是 B 对象，所以 ba 和 b 的调用结果一样
    A ba = new B();
    ba.show(c); // A.show(C)
    ba.show(d); // A.show(C)
}
```

**2. 重载（Overload）**

存在于同一个类中，指一个方法与已经存在的方法名称上相同，但是参数类型、个数、顺序至少有一个不同。

应该注意的是，返回值不同，其它都相同不算是重载。

```java
class OverloadingExample {
    public void show(int x) {
        System.out.println(x);
    }

    public void show(int x, String y) {
        System.out.println(x + " " + y);
    }
}
public static void main(String[] args) {
    OverloadingExample example = new OverloadingExample();
    example.show(1);
    example.show(1, "2");
}
```

### 八，反射

*  `Class.forName("com.mysql.jdbc.Driver")` 就是使用了类加载机制用于实例化一个对象
* Class类和java.lang.reflect类一起对反射通过了支持，其中reflect类中包括三个类
  * **Field**：可以使用set()和get()方法读取或谢盖Field对象关联的字段
  * **Method**：可以使用invoke()方法调用与其关联的方法
  * **Constructor**：可以通过其构造函数调用newInstance()来创建一个新的对象

### 九，异常

* Throwable 可以用来表示任何可以作为异常抛出的类，分为两种： **Error** 和 **Exception**。其中 Error 用来表示 JVM 无法处理的错误，Exception 分为两种：

- **受检异常** ：需要用 try...catch... 语句捕获并进行处理，并且可以从异常中恢复；
- **非受检异常** ：是程序运行时错误，例如除 0 会引发 Arithmetic Exception，此时程序崩溃并且无法恢复。

### 十，泛型

* 在java程序中泛型的使用十分广泛，例如集合中就有广泛使用泛型
* java中的泛型是一种伪泛型，在编译之后就会确定类型

### 十一，注解

* 注解+反射，是框架的灵魂，在java中使用自定义注解是很有必要的

