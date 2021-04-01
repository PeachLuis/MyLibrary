# 一、面向对象

## 1.1 多态

面向对象编程的三大特性：

- 封装：隐藏类的内部实现机制。
- 继承：重用父类代码，为多态做铺垫。
- 多态：程序中定义的引用变量所指向的具体类型和通过该引用变量发出的方法调用在编程时并不确定，而是在程序运行期间才确定。

实现多态的三个必要条件：

- 继承：在多态中必须存在有继承关系的子类和父类。
- 重写：子类对父类中的某些方法进行重新定义，在调用这些方法时就会调用子类的方法。
- 向上转型：将父类引用指向子类对象，只有这样，该引用才具备调用子类方法的能力。

实现形式：

- 基于继承实现的多态。
- 基于接口实现的多态。



## 1.2 父类静态方法能不能被子类重写

父类的静态方法可以被子类继承，但是不能被子类重写。

当子类声明了一个与父类相同的静态方法时，只能称为**隐藏**。

- 父类

```java
/**
 * @author lizejun
 **/
public class Parent {
    
    public static void staticMethod() {
        System.out.println("Parent Static Method");
    }
    
    public void method() {
        System.out.println("Parent Method");
    }
}
```

- 子类

```java
/**
 * @author lizejun
 **/
public class Child extends Parent {

    public static void staticMethod() {
        System.out.println("Child Static Method");
    }

    public void method() {
        System.out.println("Child Method");
    }
}
```

- 示例

```java
/**
 * @author lizejun
 */
public class MainApp {

    public static void main(String[] args) {
        Parent parent = new Child();
        parent.method();
        parent.staticMethod();
        
        Child child = new Child();
        child.method();
        child.staticMethod();
    }
}
```

- 运行结果。

```java
Child Method
Parent Static Method
Child Method
Child Static Method
```



# 二、`Object`类相关

## 2.1 Java 中 ==、equals 和 hashCode 的区别

**1. ==**

在`Java`中，分为基本数据类型和复合数据类型（引用数据类型），基本数据类型包括`byte`、`short`、`char`、`int`、`long`、`float`、`double`、`boolean`这八种。

- 对于基本数据类型，`==`比较的是它们的值。
- 对于复合数据类型（引用数据类型），比较的是它们在内存中的存放地址，即比较的是否是同一个对象。

**2. equals**

`Object`中`equals`默认的实现是比较两个对象是不是`==`，和`==`的效果是相同的。

```java
    public boolean equals(Object obj) {
        return (this == obj);
    }
```

而有些时候，对于两个不同的对象，我们又需要提供 **逻辑** 上是否相等的判断方法，这时候就需要重写`equals`方法。`Java`提供的某些类已经重写了`equals`方法，用于判断"相等"的逻辑，例如`Integer`，而在我们自己设计的类中，也应该重写equals方法。

```java
    public boolean equals(Object obj) {
        if (obj instanceof Integer) {
            return value == ((Integer)obj).intValue();
        }
        return false;
    }
```

**3. hashCode**

`hashCode`的目的是用于在对象进行散列的时候作为`key`输入，保证散列的存取性能。`Object`的默认`hashCode`实现为在对象的内存地址上经过特定的算法计算出。（Obejct类的hashCode方法如果传入null，则会报错；Objects类的equals方法传入null会返回0）

 由此可见，`equals`和`hashCode`的其实没有什么关系。但是由于`HashSet/HashMap`容器的存在，又需要保证：

- 对于`equals`相等两个对象，其`hashCode`返回的值一定相等
- 对于`equals`不同的对象要尽量做到`hashCode`不同。

假如我们只重写了`equals`方法，而没有重写`hashCode`方法，就会导致逻辑上相等的两个`key`，放在了容器中的不同位置。

另外，由于String的存放是采取共享机制，所以两个相等的字符串，返回的hashCode是相同的。



## 2.2 Integer

**1. 存储原理**

- `int`属于基本数据类型，存储在栈中。
- `Integer`属于复合数据类型，引用存储在栈中，引用所指向的对象存储在堆中。

**2. 缺省值**

- `0`
- `null`

**3. int 与 Integer 之间的比较**

```java
//基本数据类型。
int a1 = 128;
//非 new 出来的 Integer。
Integer a2 = 128;
//new 出来的 Integer。
Integer a3 = new Integer(128);
```

对于整型值相等的Integer如果使用 `==` 来比较：

- 非`new`出来的`Integer`与`new`出来的`Integer`不相等，前者指向存放它的常量池（数值位于`-128`到`127`之间）或者堆，后者指向堆中的另外一块内存。
- 两个都是非`new`出来的`Integer`，如果在`-128`到`127`之间，返回的是`true`，否则返回的是`false`，因为`Java`在编译`Integer a2 = 128`的时候，会翻译成`Integer.valueOf(128)`，而`valueOf`函数会对`-128`到`127`之间的数进行缓存。

```java
    public static Integer valueOf(int i) {
        //low = -128, high = 127.
        if (i >= IntegerCache.low && i <= IntegerCache.high)
            return IntegerCache.cache[i + (-IntegerCache.low)];
        return new Integer(i);
    }
```

- 两个都是`new`出来的，返回`false`。
- `int`与`Integer`相比，都为`true`，因为会把`Integer`自动拆箱为`int`再去比较

不过如果使用Integer的equals方法比较的话，返回值是相等的，因为比较的intValue



## 2.3 String

### 2.3.1 new String 和直接赋值的区别

`new String`和直接赋值的区别：

- `String str1 = "ABC"`，可能创建一个或者不创建对象，如果`ABC`这个字符串在**常量池**中已经存在了，那么`str1`直接指向这个常量池中的对象。
- `String str2 = new String("ABC")`，至少创建一个对象。一定会在**堆**中创建一个`str2`中的`String`对象，它的`value`是`ABC`，如果`ABC`这个字符串在**常量池**中不存在，会在池中创建一个对象。

**例子 1**

```java
String s ="a" + "b" + "c" + "d"
```

只创建了一个对象，在编译器在编译时优化后，相当于直接定义了一个`abcd`的字符串。

**例子 2**

```java
String ab = "ab";                                          
String cd = "cd";                                       
String abcd = ab + cd;                                      
String s = "abcd";  
```

`ab`和`cd`存储的是两个常量池中的对象，当执行`ab + cd`时，首先会在堆中创建一个`StringBuilder`类，同时用`ab`指向的字符串对象完成初始化，然后调用`append`方法完成对`cd`指向字符串的合并操作，接着调用`StringBuilder`的`toString`方法在堆中创建一个`String`对象，最后将刚生成的`String`对象的地址存放在局部变量`abcd`中。



### 2.3.2 String、StringBuilder、StringBuffer 的区别

**对比**

- `String`中的是常量数组，只能被赋值一次。
- 在编译阶段就能够确定的字符串常量，没有必要创建`String/StringBuffer`对象，直接使用字符串常量的`+`效率更高。
- `StringBuffer`中的`value[]`是一个很普通的数组，而且可以通过`append`方法将新字符串加入到末尾，改变内容和大小。
- `StringBuffer`允许多线程操作，其很多方法都被关键字`synchronized`修饰，而`StringBuilder`则不是，如果不考虑线程安全，`StringBuilder`应该是首选。

**注意点**

- 不停地创建对象是程序低效的原因，因此我们应该尽可能保证相同的字符串在堆中只创建一个`String`对象。
- 当调用`String`的`intern`时，如果常量池已经有了当前`String`的值，那么返回这个常量指向的地址；如果没有，则将`String`值加入到常量池中。
- [String、StringBuffer、StringBuilder 详细对比](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fls5718%2Farticle%2Fdetails%2F51899027)



### 2.3.3 String 为什么要设计成不可变类

**常量池的需要**

字符串常量池是`Java`内存的一个特殊区域是独立的内存空间管理，当创建一个`String`对象时，假如字符串已经存在于常量池中，则不会创建新的对象，而是直接引用已经存在的对象。

```java
String s1 = "abc";
String s2 = "abc";
```

`s1`和`s2`指向常量池中的同一个对象`abc`，如果`String`是可变类，`s1`对其的修改将会影响到`s2`。

**HashCode 缓存的需要**

因为字符串不可变，在创建的时候`HashCode`就被缓存，不需要重新计算。

**多线程安全**

由多个线程之间共享，不需要同步处理。

**如何实现不可变**

- 私有成员变量
- `public`的方法都是复制一份数据
- `String`是`final`，因此不可继承
- 构造函数深拷贝，进行`copy`而不是直接将`value[]`赋值给内部变量。



## 2.4 序列化 & 反序列化

### 2.4.1 概览

对象的序列化是把**`Java`对象**转化为**字节序列**并存储至一个**存储媒介**（硬盘或者内存）的过程，反序列化则是把字节序列恢复为`Java`对象的过程，但它们**仅处理`Java`变量而不处理方法。**

序列化的原因：

- 永久性保存对象，保存对象的字节序列到本地文件中。`Serializable`
- 通过序列化对象在网络中传递对象。`Serializable`
- 通过序列化在进程间传递对象。`Parcelable`

两种序列化的区别：

- `Serializable`只需要对某个类以及它的属性实现`Serializable`接口即可，它的缺点是使用了**反射**，序列化的过程比较慢，这种机制会在序列化的时候创建许多的临时对象，容易引发频繁的`gc`（垃圾回收）。
- 而`Parcelable`是**`Android`平台特有**的，在使用内存的时候性能更好，**但`Parcelable`不能使用在要将数据存储在磁盘的情况下**，因为`Parcelable`不能很好的保证数据的持续性在外界有变化的情况。



### 2.4.2 序列化在Android平台上的应用

1. **通过`Intent`传递复杂对象：**

   > Intent支持传递的数据类型包括：
   >
   > - 基本类型的数据、及其数组。
   > - `String/CharSequence`类型的数据、及其数组。
   > - `Parcelable/Serializable`，及其数组/列表数据。

2. **使用`SharePreference`存储复杂对象**



### 2.4.3 `Serializable`和`Parcelable`具体操作

**1. 使用`Serializable`的读写操作**

首先定义我们要序列化的对象。

```java
public class SBook implements Serializable {            
     public int id;    
     public String name;
}
```

进行读写操作：

```csharp
    private void readSerializable() {
        ObjectInputStream object = null;
        try {
            FileInputStream out = new FileInputStream(Environment.getExternalStorageDirectory() + "/sbook.txt");
            object = new ObjectInputStream(out);
            SBook book = (SBook) object.readObject();
            if (book != null) {
                Log.d(TAG, "book, id=" + book.id + ",name=" + book.name);
            } else {
                Log.d(TAG, "book is null");
            }
        } catch (Exception e) {
            Log.d(TAG, "readSerializable:" + e);
        } finally {
            try {
                if (object != null) {
                    object.close();
                }
            } catch (Exception e) {
                Log.d(TAG, "readSerializable:" + e);
            }
        }
    }

    private void writeSerializable() {
        SBook book = new SBook();
        book.id = 1;
        book.name = "SBook";
        ObjectOutputStream object = null;
        try {
            FileOutputStream out = new FileOutputStream(Environment.getExternalStorageDirectory()  + "/sbook.txt");
            object = new ObjectOutputStream(out);
            object.writeObject(book);
            object.flush();
        } catch (Exception e) {
            Log.d(TAG, "writeSerializable:" + e);
        } finally {
            try {
                if (object != null) {
                    object.close();
                }
            } catch (Exception e) {
                Log.d(TAG, "writeSerializable:" + e);
            }
        }
    }
```



**2. 使用`Parcelable`的读写操作**

定义序列化对象：

```java
public class PBook implements Parcelable {

    public int id;
    public String name;

    public PBook(int id, String name) {
        this.id = id;
        this.name = name;
    }

    private PBook(Parcel in) {
        id = in.readInt();
        name = in.readString();
    }

    public static final Parcelable.Creator<PBook> CREATOR = new Parcelable.Creator<PBook>() {

        @Override
        public PBook[] newArray(int size) {
            return new PBook[size];
        }

        @Override
        public PBook createFromParcel(Parcel source) {
            return new PBook(source);
        }
    };

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeInt(id);
        dest.writeString(name);
    }
}
```

写入和读取：

```cpp
    private Intent writeParcelable() {
        PBook tony = new PBook(1, "tony");
        PBook king = new PBook(2, "king");
        ArrayList<PBook> list = new ArrayList<>();
        list.add(tony);
        list.add(king);
        Intent intent = new Intent();
        intent.putParcelableArrayListExtra("PBook", list);
        return intent;
    }

    private void readParcelable(Intent intent) {
        if (intent != null) {
            ArrayList<PBook> list = intent.getParcelableArrayListExtra("PBook");
            if (list != null) {
                for (PBook book : list) {
                    Log.d(TAG, "readParcelable, id=" + book.id + ", name=" + book.name);
                }
            }
        }
    }
```

如果使用Android的Intent传输数据的话，获取数据方法为：

- Serializable：

  ```java
  Person person = (Person) getIntent().getSerializableExtra("person_data");
  ```

- Paracelable：

  ```java
  Person person = (Person) getIntent().getParacelable("person_data");
  ```

  

### 2.4.4 使用SharedPreference存储复杂对象

由于`Paracelable`不能用于硬盘存储，所以只能使用SharedPreference处理实现了Serializable的类。

```java
    private void writeSP() {
        SBook book = new SBook();
        book.id = 2;
        book.name = "sp";
        SharedPreferences sp = getSharedPreferences("SBookSP", MODE_PRIVATE);
        ByteArrayOutputStream os = new ByteArrayOutputStream();
        try {
            ObjectOutputStream object = new ObjectOutputStream(os);
            object.writeObject(book);
            String base64 = new String(Base64.encode(os.toByteArray(), Base64.DEFAULT));
            SharedPreferences.Editor editor = sp.edit();
            editor.putString("SBook", base64);
            editor.apply();
        } catch (Exception e) {}
    }
    
    private void readSP() {
        SharedPreferences sp = getSharedPreferences("SBookSP", MODE_PRIVATE);
        String sbook = sp.getString("SBook", "");
        if (sbook.length() > 0) {
            byte[] base64 = Base64.decode(sbook.getBytes(), Base64.DEFAULT);
            ByteArrayInputStream is = new ByteArrayInputStream(base64);
            try {
                ObjectInputStream object = new ObjectInputStream(is);
                SBook book = (SBook) object.readObject();
                if (book != null) {
                    Log.d(TAG, "readSP, id=" + book.id + ", name=" + book.name);
                }
            } catch (Exception e) {

            }
        }
    }
```



# 三、重要关键字

## 3.1 final

`final`可以用于以下四个地方：

- 变量：静态和非静态
- 定义方法的参数
- 定义方法
- 定义类

### 3.1.1 变量

#### 静态变量

- 如果`final`修饰的是一个基本类型，就表示这个变量被赋予的值是不可变的。
- 如果`final`修饰的是一个对象，就表示这个变量被赋予的引用是不可变的。

#### 非静态变量

**被`final`修饰的变量必须被初始化**，初始化的方式有以下几种：

- 在定义的时候初始化
- 非静态`final`变量在初始化块中初始化，不可在静态初始化块中初始化
- 静态`final`变量可以在静态初始化块中初始化。
- 非静态`final`变量可以在类的构造器中初始化，但是静态`final`变量不可以

### 3.1.2 方法

不可以被子类重写，但是不影响被子类继承。

### 3.1.3 类

不允许被继承。



## 3.2 static

主要作用：方便在没有创建对象的情况下来调用（方法/变量（经常为常量））

### 3.2.1 static 方法

- 静态方法不依赖于任何对象就可以访问，因此对于静态方法来说，是没有`this`的。（但是可以在该类中通过this访问static变量）
- 静态方法中不能访问类的非静态成员变量和非静态成员方法。
- 非静态方法可以访问静态方法和变量

### 3.2.2 static 变量

- 静态变量被所有的对象所**共享**，在内存中只有一个副本，它当且仅当在类初次加载时被初始化。
- 非静态变量是对象所拥有的，在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。
- `static`成员变量的初始化顺序按照定义的顺序进行初始化。
- `static`变量仅用于类的实例域，**不能用于局部变量**

### 3.2.3 static 代码块

- `static`块可以用来优化程序性能
- `static`块可以置于类的任何地方，类中可以有多个`static`块。
- 在类初次被加载的时候，会按照`static`块的顺序来执行每个`static`块，并且**只执行一次**。

### 3.2.4 执行顺序

[Java中的static关键字解析](https://www.cnblogs.com/dolphin0520/p/3799052.html)

另外一种情况：

```java
public class Test {
    static{
        System.out.println("static block");
    }

    {
        System.out.println("block");
    }

    public Test() {
        System.out.println("constructor");
    }

    public static void main(String[] args) {
        System.out.println("main");
        new Test();
    }
}
```

输出：

```
static block
main
block
constructor
```



# 四、内部类

## 4.1 定义

内部类的定义：在一个外部类的内部再定义一个类。

## 4.2 分类

- 成员内部类：作为外部类的成员，可以直接使用外部类的所有成员和方法。
- 静态内部类：声明为`static`的内部类，成员内部类不能有`static`数据和`static`方法，但嵌套内部类可以。
- 局部内部类：内部类定义在方法和作用域内。只在该方法或条件的作用域内才能使用，退出作用域后无法使用。
- 匿名内部类：匿名内部类有几个特点：不能加访问修饰符；当所在方法的形参需要被内部类里面使用时，该形参必须为`final`。

## 4.3 作用

### 4.3.1 实现隐藏

外部顶级类即类名和文件名相同的只能使用`public`和`default`修饰，但内部类可以是`static`、`public/default/protected/private`。

首先，定义内部类需要实现的接口：

```java
public interface InnerInterface {
    void call();
}
```

再定义一个包装类：

```java
public class Outer {

    private class InnerImpl implements InnerInterface {

        @Override
        public void call() {
            System.out.println("call inner");
        }
    }

    public InnerInterface getInnerInterface() {
        return new InnerImpl();
    }
}
```

由于我们将`InnerInterface`的实现类声明为了`private`，因此外部并不知道它的存在，也就达到了隐藏的目的。

```java
public class MainApp {

    public static void main(String[] args) {
        Outer outer = new Outer();
        InnerInterface inner = outer.getInnerInterface();
        inner.call();
    }
}
```



### 4.3.2 无条件地访问外部类当中的元素

```java
public class Outer {
    
    //外部类的私有变量。
    private int outerSelfValue = 0;

    private class InnerImpl implements InnerInterface {

        @Override
        public void call() {
            //内部类可以无条件地访问。
            System.out.println("call inner, outerValue=" + outerSelfValue);
        }

    }

    public InnerInterface getInnerInterface() {
        return new InnerImpl();
    }
}
```

这仅限于非静态内部类，它和静态内部类的区别是：

- 静态内部类没有指向外部的引用
- **在任何非静态内部类中，都不能有静态变量、静态方法或者静态内部类。**
- 创建非静态内部类，必须要通过外部类来创建，例如`Outer.InnerImpl outer = new Outer().new InnerImpl();`；静态内部类则可以直接创建，`Outer.InnerImpl outer = new Outer.InnerImpl();`
- 静态内部类只可以访问外部类的静态方法和静态变量。



### 4.3.3 实现多重继承

由于`Java`不允许多重继承，因此假如我们希望一个类同时具备其它两个类的功能时，就可以采用内部类来实现。

实现乘法的子类：

```java
public class MultiCalculator {

    public int multi(int a, int b) {
        return a * b;
    }
}
```

实现加法的子类：

```java
public class PlusCalculator {

    public int add(int a, int b) {
        return a;
    }
}
```

通过内部类实现多重继承

```java
public class Calculator extends PlusCalculator {

    class MultiCalculatorImpl extends MultiCalculator {

        @Override
        public int multi(int a, int b) {
            return super.multi(a, b);
        }
    }

    public int multi(int a, int b) {
        return new MultiCalculatorImpl().multi(a, b);
    }
}
```



### 4.3.4 避免修改接口而实现同一个类中两种同名方法的调用

用于解决下面的困境：一个需要继承另一个类，还要实现一个接口，而继承的类和接口里面有两个同名的方法。那么我们调用该方法的时候，究竟是父类的，还是实现的接口呢，这时候就可以使用内部类来解决这一问题。

- 需要继承的子类中有`call`方法

```java
public class BaseOuter {

    public void call() {
        System.out.println("call baseOuter");
    }
}
```

- 需要实现的接口，同样有`call`方法

```java
public interface InnerInterface {
    void call();
}
```

- 采用内部类的方式避免出现困惑

```java
public class Outer extends BaseOuter {

    private class InnerImpl implements InnerInterface {

        @Override
        public void call() {
            //内部类可以无条件地访问。
            System.out.println("call inner");
        }

    }

    public InnerInterface getInnerInterface() {
        return new InnerImpl();
    }
}
```

- 调用方式

```java
/**
 * @author lizejun
 */
public class MainApp {

    public static void main(String[] args) {
        Outer outer = new Outer();
        //1. 调用的是继承父类的接口。
        outer.call();
        //2. 调用的实现接口的方法。
        outer.getInnerInterface().call();
    }
}
```



## 4.4 应用场景

[幕后英雄的用武之地——浅谈 Java 内部类的四个应用场景](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fhivon%2Farticle%2Fdetails%2F606312)

- 除了它的外部类，不再被其它的类使用
- 解决一些非面向对象的语句块

```java
public interface InnerWorker {
    void work();
}
```

```java
/**
 * @author lizejun
 **/
public class Factory {

    public void doWork(InnerWorker worker) {
        try {
            worker.work();
        } catch (Exception exception) {
            System.out.println("exception!");
        } finally {
            System.out.println("finally!");
        }
    }
}
```



```java
/**
 * @author lizejun
 */
public class MainApp {

    public static void main(String[] args) {
        Factory factory = new Factory();
        factory.doWork(new InnerWorker() {

            @Override
            public void work() {
                System.out.println("work1 work");
            }
        });
        factory.doWork(new InnerWorker() {

            @Override
            public void work() {
                System.out.println("work2 work");
            }
        });
    }
}
```

- 一些多算法场合
- 适当使用内部类，使得代码更加灵活和具有扩展性

```java
public interface Shape {
    void draw();
}
```

```java
public abstract class ShapeFactory {

    private static HashMap<String, ShapeFactory> factories = new HashMap();

    public static void addFactory(String id, ShapeFactory factory) {
        factories.put(id, factory);
    }

    public static Shape createShape(String id) {
        if (!factories.containsKey(id)) {
            try {
                Class.forName(id);
            } catch (Exception e) {}
        }
        return factories.get(id).create();
    }

    protected abstract Shape create();

}
```

```java
public class MainApp {

    public static void main(String[] args) {
        Shape shape = ShapeFactory.createShape(Circle.ID);
        shape.draw();
    }
}
```

## 4.5 内部类和闭包

闭包就是把函数以及变量包起来，使得变量的生存周期延长，闭包跟面向对象是一棵树上的两条枝，实现的功能是等价的。

涉及到闭包的两种内部是：局部内部类和匿名内部类。当它们引用外部变量时，**外部的变量需要是`final`的。**

以下面这个例子为例，定义一个内部类的接口：

```java
public interface InnerInterface {
    void call();
}
```

```java
public class InnerClose {
    
    public void doClose(final int a) {
        InnerInterface inner = new InnerInterface() {
            
            @Override
            public void call() {
                System.out.println("a=" + a);    
            }
        };
        inner.call();
    }
}
```

```java
public class MainApp {

    public static void main(String[] args) {
        InnerClose close = new InnerClose();
        close.doClose(1);
    }
}
```

在编译之后，局部内部类会生成独立的`InnerClose$1.class`文件，而变量`a`是方法级别的，方法运行完变量就销毁了，而局部内部类对象还可能一直存在，不会随着方法运行结束就马上被销毁。这时候就会出现，局部内部类对象需要访问一个已经不存在的局部变量`a`。

因此，通过将变量声明为`final`，编译器会将`final`局部变量复制一份，复制品作为局部内部类中的成员，这样，当局部内部类访问局部变量时，其实真正访问的是这个局部变量的复制品。

由于被`final`修饰的变量赋值后不能再修改，所以就保证了复制品与原始变量的一致，就好像是局部变量的 **生命期变长了**，这就是`Java`的闭包。

[匿名内部类为什么访问外部类局部变量必须是 final 的](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.cnblogs.com%2FDarrenChan%2Fp%2F5738957.html)



# 五、抽象类 & 接口

## 5.1 区别

- 抽象类和接口都不能被实例化。
- 抽象类要被子类继承，接口要被类实现。
- 接口只能做方法的声明，抽象类可以做方法的声明，也可以做方法的实现。
- **接口里定义的变量只能是公共的静态常量，抽象类中的变量可以是普通变量**。
- 抽象类里的抽象方法必须全部被子类实现；接口的接口方法必须全部被子类实现，否则只能为抽象类。
- 抽象类里可以没有抽象方法。
- 如果一个类里有抽象方法，那么这个类只能是抽象类。
- 抽象方法要被实现，所以不能是静态的，也不能是私有的。
- 接口可继承接口，并可多继承接口，但类只能单继承。

## 5.2 应用场景

#### 抽象类

**在既需要统一的接口，又需要实例变量或缺省方法的情况下**，可以使用：

- 定义了一组接口，但又不想强迫每个实现类都必须实现所有的接口。
- 某些场合下，只靠纯粹的接口不能满足类与类之间的协调，**还需要类中表示状态的变量来区别不同的关系**。
- 规范了一组相互协调的方法，其中一些方法是共同的，与状态无关的，可以共享的，无需子类分别实现；而另一些方法却需要各个子类根据自己特定的状态来实现特定的功能。

#### 接口

- 类与类之间需要特定的接口协调，而不在乎其如何实现。
- 需要将一组类视为单一的类，而调用者只通过接口来与这组类发生联系。



# 六、编码

## 6.1 为什么要编码

- 计算机中存储信息的最小单元是`8bit`，所以能表示的字符范围是`0~255`个。
- 要表示的符号太多，无法用一个字节来完全表示。
- 要解决这个矛盾必须要一个新的数据结构`char`，从`char`到`byte`必须编码。

## 6.2 编码方式

### ASCII 码

`ASCII`码总共有`128`个，用一个字节的低`7`位表示。

### ISO-8859-1

在`ASCII`码基础上制定了一系列标准来扩展`ASCII`编码，其仍然是单字节编码，总共能表示`256`个字符。

### GB2312

双字节编码，总的范围是`A1~F7`，从`A1~A9`是符号区，总共包含`682`个符号；从`B0~F7`是汉字区，包含`6763`个汉字。

### GBK

扩展`GB2312`，加入更多的汉字，其编码范围是`8140~FEFE`，和`GB2312`兼容。

### GB18030

我国的强制标准，可能是单字节、双字节或者四字节编码，与`GB2312`兼容。

### Unicode 编码集

`ISO`试图创建一个全新的语言字典，将所有的语言互相翻译。`String`在内存中 **不需要编码格式**，它只是一个`Unicode`字符串而已。只有当字符串需要在网络中传输或要被写入文件时，才需要编码格式。

- `UTF-16`
   `UTF-16`具体定义了`Unicode`字符在计算机中的存取方法，它用两个字节表示`Unicode`转化格式。

- ```
  UTF-8
  ```

  ```
  UTF-16
  ```

  的缺点在于很大部分字符仅用一个字节就可以表示，目前却需要使用两个，而

  ```
  UTF-8
  ```

  采用了变长技术，不同类型的字符可以由

  ```
  1~6
  ```

  个字节组成。

  - 如果一个字节，最高位为`0`，表示这是一个`ASCII`字符。
  - 如果一个字节，以`11`开头，连续的`1`个数表示这个字符的字节数。
  - 如果一个字节，以`10`开始，表示它不是首字节，需要向前查找才能得到当前字符的首字节。



```java
String s = "严"; 
//编码。
byte[] b = s.getBytes("UTF-8");
//解码。 
String n = new String(b,"UTF-8"); 
```

## 6.3 对比

- `GB2312`与`GBK`编码规则类似，但是`GBK`范围更大，它能处理所有汉字字符。
- `UTF-16`和`UTF-8`都是处理`Unicode`编码，`UTF-16`效率更高，它适合在本地磁盘和内存之间使用。
- `UTF-16`不适合在网络之间传输，因为网络传输容易损坏字节流，`UTF-8`更适合网络传输，对`ASCII`字符采用单字节存储，单字节损毁不会影响后面其它字符。

## 6.4 参考文章

- [(1) Java 中的字符编码方式](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.cnblogs.com%2Fliujinhong%2Fp%2F5995946.html)
- [(2) Java 几种常见的编码格式](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fqq_35038153%2Farticle%2Fdetails%2F79690608)
- [Unicode, UTF-8, UTF-16](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fguxiaonuan%2Farticle%2Fdetails%2F78678043)



# 七、异常

`Java`中定义了许多异常类，并定义了`Throwable`作为所有异常的超类，将异常划分为两类`Error`和`Exception`。

- `Error`：程序中无法处理的错误，例如`NoClassDefFoundError`、`OutOfMemory`等，当此类错误发生时，**`JVM`将终止进程**。
- `Exception`：程序本身可以处理的异常。
  - 运行时异常，`RuntimeException`及其子类，表示`JVM`在运行时可能出现的错误，例如空指针、数组越界等，一般是由逻辑错误引起。
  - 受检异常：除`RuntimeException`及其子类的异常。编译器会检查此类异常，并提示你处理本类异常 - 要么使用`try-catch`捕获，要么使用`throws`语句抛出，否则编译不通过。



# 八、注解

## 8.1 什么是注解

注解可以向编译器、虚拟机等解释说明一些事情。举一个最常见的例子，当我们在子类当中**覆写**父类的`aMethod`方法时，在子类的`aMethod`上会用`@Override`来修饰它，反之，如果我们给子类的`bMethod`用`@Override`注解修饰，但是在它的父类当中并没有这个`bMethod`，那么就会报错。这个`@Override`就是一种注解，它的作用是告诉编译器它所注解的方法是重写父类的方法，这样编译器就会去**检查父类是否存在这个方法**。
 注解是用来描述`Java`代码的，它既能被编译器解析，也能在运行时被解析。



## 8.2 元注解

元注解是**描述注解的注解**，也是我们编写自定义注解的基础，比如以下代码中我们使用`@Target`元注解来说明`MethodInfo`这个注解只能应用于对方法进行注解：

```kotlin
@Target(ElementType.METHOD)
public @interface MethodInfo {
    //....
}
```

下面我们来介绍4种元注解，我们可以发现这四个元注解的定义又借助到了其它的元注解：

### 8.2.1 `Documented`

当一个注解类型被`@Documented`元注解所描述时，那么无论在哪里使用这个注解，都会被`Javadoc`工具文档化，我们来看以下它的定义：

```kotlin
@Documented 
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Documented {
    //....
}
```

- 定义注解时使用`@interface`关键字：
- `@Document`表示它本身也会被文档化；
- `Retention`表示`@Documented`这个注解能保留到运行时；
- `@ElementType.ANNOTATION_TYPE`表示`@Documented`这个注解只能够被用来描述注解类型。

### 8.2.2 `Inherited`

表明被修饰的注解类型是自动继承的，若一个注解被`Inherited`元注解修饰，则当用户在一个类声明中查询该注解类型时，若发现这个类声明不包含这个注解类型，则会自动在这个类的父类中查询相应的注解类型。

我们需要注意的是，用`inherited`修饰的注解，它的这种自动继承功能，只能对**类**生效，对方法是不生效的。也就是说，如果父类有一个`aMethod`方法，并且该方法被注解`a`修饰，那么无论这个注解`a`是否被`Inherited`修饰，**只要我们在子类中覆写了`aMethod`**，子类的`aMethod`都不会继承父类`aMethod`的注解，反之，如果我们没有在子类中覆写`aMethod`，那么通过子类我们依然可以获得注解`a`。

```kotlin
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Inherited {
    //....
}
```

### 8.2.3 `Retention`

这个注解表示一个注解类型会被保留到什么时候，它的原型为：

```kotlin
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Retention { 
    RetentionPolicy value();
}
```

其中，`RetentionPolicy.xxx`的取值有：

- `SOURCE`：表示在编译时这个注解会被移除，不会包含在编译后产生的`class`文件中。
- `CLASS`：表示这个注解会被包含在`class`文件中，但在运行时会被移除。
- `RUNTIME`：表示这个注解会被保留到运行时，我们可以在运行时通过反射解析这个注解。

### 8.2.4 `Target`

这个注解说明了被修饰的注解的应用范围，其用法为：

```kotlin
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Target { 
    ElementType[] value();
}
```

`ElementType`是一个枚举类型，它包括：

- `TYPE`：类、接口、注解类型或枚举类型。
- `PACKAGE`：注解包。
- `PARAMETER`：注解参数。
- `ANNOTATION_TYPE`：注解 注解类型。
- `METHOD`：方法。
- `FIELD`：属性（包括枚举常量）
- `CONSTRUCTOR`：构造器。
- `LOCAL_VARIABLE`：局部变量。



## 8.3 常见注解

### 8.3.1 `@Override`

```kotlin
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {}
```

**重写**标志

告诉编译器被修饰的方法是重写的父类中的相同签名的方法，编译器会对此做出检查，若发现父类中不存在这个方法或是存在的方法签名不同，则会报错。

### 8.3.2 `@Deprecated`

```kotlin
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})
public @interface Deprecated {}
```

已废弃，不建议使用这些被修饰的程序元素。

### 8.3.3 `@SuppressWarnings`

```css
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings { 
  String[] value();
}
```

告诉编译器忽略指定的警告信息。



## 8.4 自定义注解

在自定义注解前，有一些基础知识：

- 注解类型是用`@interface`关键字定义的。
- 所有的方法均没有方法体，且只允许`public`和`abstract`这两种修饰符号，默认为`public`。
- 注解方法只能返回：原始数据类型，`String`，`Class`，枚举类型，注解，它们的一维数组。

下面是一个例子：

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Inherited
public @interface MethodInfo { 
  String author() default "absfree"; 
  String date(); 
  int version() default 1;
}
```



## 8.5 注解的解析

好难！



# 九、容器

## 9.1 前言

Java容器集合框架：

![img](https://upload-images.jianshu.io/upload_images/1949836-d8443c199292f5bd?imageMogr2/auto-orient/strip|imageView2/2/w/1108/format/webp)

Java集合分为两个大类：Collection和Map

另外的分类方法：

1. 按作用分：

   - 集合：Collection
   - 键值对：Map

2. 按是否可重复：

   - 可重复：List
   - 不可重复：Set

3. 按有序分：

   - 有序：Set，Map
   - 无序：List

   >  Collection都可排序，使用Collections.sort方法来排序，不过如果Collection中如果有null值，则会爆空指针异常

4. 按线程安全分（）：

   - 安全：Vector、ConcurrentHashMap、HashTable
   - 不安全：ArrayList、LinkedList、HashMap、TreeMap、LinkedHashMap

   > List可通过`Collections.synchronizedList(List T)`函数返回一个线程安全的类。
   >
   > ArrayList在多线程也可使用并发包下线程安全的`CopyOnWriteArrayList`类。

5. 按实现分：

   - 数组实现：ArrayList、Vector
   - 链表实现：LinkedList（双向链表）

### 9.1.1 Collection

`Collection`是`List`和`Set`抽象出来的接口，它包含了这些集合的基本操作。

**(1) List**

`List`接口通常表示一个列表（数组、队列、链表，栈等），其中的元素可以重复，常用的实现类为`ArrayList`、`LinkedList`和`Vector`。

**(2) Set**

`Set`接口通常表示一个集合，集合中的元素不允许重复（通过`hashCode`和`equals`函数保证），常用的实现类有`HashSet`和`TreeSet`，`HashSet`是通过`Map`中的`HashMap`来实现的，而`TreeSet`则是通过`Map`中的`TreeMap`实现的，另外`TreeSet`还实现了`SortedSet`接口，因此是有序的集合。

**(3) List 和 Set 的区别**

- `Set`接口存储的是无序的、不重复的数据
- `List`接口存储的是~~有序的、~~可以重复的数据
- `Set`检索效率低，删除和插入效率高，插入和删除不会引起元素位置改变。
- `List`查找元素效率高，删除和插入效率低，`List`和数组类似，可以动态增长，根据实际存储的长度自动增长`List`的长度。

**(4) 使用的设计模式**

抽象类`AbstractCollection`、`AbstractList`和`AbstractSet`分别实现了`Collection`、`List`和`Set`接口，这就是在`Java`集合框架中用的很多的适配器设计模式，用这些抽象类去实现接口，在抽象类中实现接口中的若干或全部方法，这样下面的一些类只需直接继承该抽象类，并实现自己需要的方法即可，而不用实现接口中的全部抽象方法。

### 9.1.2 Map

`Map`是一个映射接口，其中的每个元素都是一个`Key-Value`键值对，同样抽象类`AbstractMap`通过**适配器模式**实现了`Map`接口的大部分函数，`TreeMap`、`HashMap`和`WeakHashMap`等实现类都通过继承`AbstractMap`来实现。

### 9.1.3 Iterator

`Iterator`是遍历集合的迭代器，它可以用来遍历`Collection`，但是不能用来遍历`Map`。`Collection`的实现类都实现了`iterator()`函数，它返回一个`Iterator`对象，用来遍历集合，`ListIterator`则专门用来遍历`List`。而`Enumeration`则是`JDK 1.0`时引入的，作用与`Iterator`相同，但它的功能比`Iterator`要少，它只能在`Hashtable`、`Vector`和`Stack`中使用。

### 9.1.4 Arrays 和 Collections

`Arrays`和`Collections`是用来操作数组、集合的两个工具类，例如在`ArrayList`和`Vector`中大量调用了`Arrays.Copyof()`方法，而`Collections`中有很多静态方法可以返回各集合类的`synchronized`版本，即线程安全的版本，当然了，如果要用线程安全的集合类，**首选`concurrent`并发包下的对应的集合类。**



## 9.2 ArrayList

`ArrayList`是基于**数组**实现的。

`ArrayList`是基于一个能动态增长的数组实现，`ArrayList`并不是线程安全的，在多线程的情况下可以考虑使用**`Collections.synchronizedList(List T)`函数返回一个线程安全的`ArrayList`类**，**也可以使用并发包下的`CopyOnWriteArrayList`类。**

`ArrayList<T>`类继承于`AbstractList<T>`，并实现了以下四个接口：

- `List<T>`
- `RandomAccess`：支持快速随机访问
- `Cloneable`：能够被克隆
- `Serializable`：支持序列化

### 9.2.1 ArrayList 的扩容

由于`ArrayList`是基于数组实现的，因此当我们通过`addXX`方法向数组中添加元素之前，都要保证有足够的空间容纳新的元素，这一过程是通过`ensureCapacityInternal`来实现的，传入的参数为所要求的数组容量：

- 如果当前数组为空，并且要求的容量小于`10`，那么将要求的容量设为`10`
- 接着尝试将数组大小扩充为当前大小的`1.5`倍
- 如果仍然无法满足要求，那么将数组大小设为要求的容量
- 如果要求的容量大于`Interger.MAX_VALUE-8`，那么调用`hugeCapacity`方法，将数组的容量设为整型的最大值
- 最后，调用`Arrays.copyOf`将原有数组中的元素复制到新的数组中。

`Arrays.copyOf`最终会调用到`System.arraycopy()`方法。该`Native`函数实际上最终调用了`C`语言的`memmove()`函数，因此它可以保证同一个数组内元素的正确复制和移动，比一般的复制方法的实现效率要高很多，很适合用来批量处理数组，`Java`强烈推荐在复制大量数组元素时用该方法，以取得更高的效率。

### 9.2.2 ArrayList 转换为静态数组

`ArrayList`中提供了两种转换为静态数组的方法：

- `Object[] toArray()`
   该方法有可能会抛出`java.lang.ClassCastException`异常，如果直接用向下转型的方法，将整个`ArrayList`集合转变为指定类型的`Array`数组，便会抛出该异常，而如果转化为`Array`数组时不向下转型，而是将每个元素向下转型，则不会抛出该异常，显然对数组中的元素一个个进行向下转型，效率不高，且不太方便。
- `T[] toArray(T[] a)`
   该方法可以直接将`ArrayList`转换得到的`Array`进行整体向下转型，且从该方法的源码中可以看出，参数`a`的大小不足时，内部会调用`Arrays.copyOf`方法，该方法内部创建一个新的数组返回，因此对该方法的常用形式如下：

```java
public static Integer[] vectorToArray2(ArrayList<Integer> v) {    
    Integer[] newText = (Integer[])v.toArray(new Integer[0]);    
    return newText;    
}   
```

### 9.2.3 元素访问方式

`ArrayList`基于数组实现，可以通过下标索引直接查找到指定位置的元素，因此**查找效率高**，但每次插入或删除元素，就要大量地移动元素，**插入删除元素的效率低**。

在查找给定元素索引值等的方法中，源码都将该元素的值分为`null`和不为`null`两种情况处理，`ArrayList`中**允许元素为`null`。**



## 9.3 LinkedList

`LinkedList`是基于双向循环链表实现的，除了可以当作链表来操作外，它还可以当作栈，队列和双端队列来使用。

`LinkedList`同样是非线程安全的，在多线程的情况下可以考虑使用`Collections.synchronizedList(List T)`函数返回一个线程安全的`LinkedList`类，`LinkedList`继承于`AbstractSequentialList`类，同时实现了以下四个接口：

- `List<T>`
- `Deque`和`Queue`：双端队列
- `Cloneable`：支持克隆操作
- `Serializable`：支持序列化

### 9.3.1 链表节点

`LinkedList的`实现是基于双向循环链表的，且头结点`voidLink`中不存放数据，所以它也不存在扩容的方法，只需改变节点的指向即可，每个链表节点包含该节点的数据，以及前驱和后继节点的引用，其定义如下所示：



```java
    private static final class Link<ET> {
        //该节点的数据。
        ET data;
        //前驱节点和后继节点。
        Link<ET> previous, next;
        Link(ET o, Link<ET> p, Link<ET> n) {
            data = o;
            previous = p;
            next = n;
        }
    }
```

### 9.3.2 查找和删除操作

当需要根据位置寻找对应节点的数据时，会先比较待查找位置和链表的大小，如果小于一半，那么从头节点的后继节点开始向**遍历**后寻找，反之则从头结点的前驱节点开始往前**遍历**寻找，因此对于**查找操作来说，它的效率很低**，但是**向头尾节点插入和删除数据的效率较高。**

注意，这种查找不同于二分查找。



## 9.4 Vector

`Vector`也是基于**数组**实现的，其容量能够动态增长。它的许多实现方法都加入了同步语句，因此是 **线程安全** 的。

`Vector`继承于`AbstractList`类，并且实现了下面四个接口：

- `List<E>`
- `RandomAccess`：支持随机访问
- `Cloneable, java.io.Serializable`：支持`Clone`和序列化。

`Vector`的实现大体和`ArrayList`类似，它有以下几个特点：

- `Vector`有四个不同的构造方法，无参构造方法的容量为默认值`10`，仅包含容量的构造方法则将容量增长量置为`0`。
- 当`Vector`的容量不足以容纳新的元素时，将进行扩容操作。首先判断容量增长值是否为`0`，如果为`0`，那么就将新容量设为旧容量的两倍，否则就设置新容量为旧容量加上容量增长值。假如新容量还不够，那么就直接设置新量容量为传入的参数。
- 在存入和读取元素时，会根据元素值是否为`null`进行处理，也就是说，`Vector`允许元素为`null`。

> ArrayList和Vetor基本上一样，**元素可重复可null，无序**
>
> ArrayList，Vector主要区别为以下几点：
> （1）：Vector是线程安全的，源码中有很多的synchronized可以看出，而ArrayList不是。导致Vector效率无法和ArrayList相比；
> （2）：ArrayList和Vector都采用线性连续存储空间，当存储空间不足的时候，ArrayList默认增加为原来的50%，Vector默认增加为原来的一倍；
> （3）：Vector可以设置capacityIncrement，而ArrayList不可以，从字面理解就是capacity容量，Increment增加，容量增长的参数。



## 9.5 HashSet

`HashSet`具有以下特点：

- 不能保证元素的排列顺序，顺序有可能发生变化
- 不是同步的
- 集合元素可以是`null`，**但只能放入一个`null`**

当向`HashSet`集合中存入一个元素时，`HashSet`会调用该对象的`hashCode()`方法来得到该对象的`hashCode`值，然后根据`hashCode`值来决定该对象在`HashSet`中存储位置。
 简单的说，`HashSet`集合判断两个元素相等的标准是两个对象通过`equals`方法比较相等，并且两个对象的`hashCode()`方法返回值相等。

注意，如果要把一个对象放入`HashSet`中，重写该对象对应类的`equals`方法，也应该重写其`hashCode()`方法。其规则是如果两个对象通过`Objects.equals`方法比较返回`true`时，其`hashCode`也应该相同。另外，对象中用作`equals`比较标准的属性，都应该用来计算`hashCode`的值。



## 9.6 TreeSet

`TreeSet`是`SortedSet`接口的唯一实现类，`TreeSet`可以确保集合元素处于排序状态。`TreeSet`支持两种排序方式，**自然排序** 和 **定制排序**，其中自然排序为默认的排序方式。

向`TreeSet`中加入的应该是同一个类的对象。`TreeSet`判断两个对象不相等的方式是两个对象通过`equals`方法返回`false`，或者通过`CompareTo`方法比较没有返回`0`。

### 9.6.1 自然排序

自然排序使用要排序元素的`CompareTo(Object obj)`方法来比较元素之间大小关系，然后将元素按照升序排列。
 `Java`提供了一个`Comparable`接口，该接口里定义了一个`compareTo(Object obj)`方法，该方法返回一个整数值，实现了该接口的对象就可以比较大小。

`obj1.compareTo(obj2)`方法如果返回`0`，则说明被比较的两个对象相等，如果返回一个正数，则表明`obj1`大于`obj2`，如果是负数，则表明`obj1`小于`obj2`。如果我们将两个对象的`equals`方法总是返回`true`，则这两个对象的`compareTo`方法返回应该返回`0`.

### 9.6.2 定制排序

自然排序是根据集合元素的大小，以升序排列，如果要定制排序，应该使用`Comparator`接口，实现`int compare(T o1,T o2)`方法。

`HashSet`和`TreeSet`d 区别：

- `TreeSet`是二叉树实现的，`Treeset`中的数据是自动排好序的，不允许放入`null`值。
- `HashSet`是哈希表实现的，`HashSet中`的数据是无序的，可以放入`null`，但只能放入一个`null`，两者中的值都不能重复，就如数据库中唯一约束。
- `HashSet`要求放入的对象**必须实现`hashCode()`方法**，放入的对象，是以`hashcode()`码作为标识的，而具有相同内容的`String`对象，`hashcode`是一样，所以放入的内容不能重复。但是同一个类的对象可以放入不同的实例 。



## 9.7 HashMap

`HashMap`是基于哈希表实现的，每一个元素都是一个`key-value`对，其内部通过单链表解决冲突问题，容量不足时，同样会自动增长。`HashMap`是**非线程安全**的，只是用于单线程环境下，多线程环境下可以采用并发包下的`ConcurrentHashMap`。

`HashMap`继承于`AbstractMap`，同时实现了`Cloneable`和`Serializable`接口，因此，它支持克隆和序列化。

### 9.7.1 HashMap 的整体结构

`HashMap`是基于**数组和链表**来实现的：

![img](https:////upload-images.jianshu.io/upload_images/1949836-4267f98804bdedf1.png?imageMogr2/auto-orient/strip|imageView2/2/w/446/format/webp)


 它的基本原理为：

- 首先根据`Key`的`hashCode`方法，计算出在数组中的坐标。

```java
//计算 Key 的 hash 值。
int hash = sun.misc.Hashing.singleWordWangJenkinsHash(key);
//根据 Key 的 hash 值和链表的长度来计算下标。
int i = indexFor(hash, table.length);
```

- 判断在数组的当前位置是否已经有元素，如果没有，那么就将`Key/Value`封装成`HashMapEntry`数据结构，并将其作为数组在该位置上的元素。否则就先从头节点开始遍历该链表，如果 **满足下面的两个条件**，那么就替换链表该节点的`Value`

```java
//Value 替换的条件
//条件1：hash 值完全相同
//条件2：key 指向同一块内存地址 或者 key 的 equals 方法返回为 true
(e.hash == hash && ((k = e.key) == key || key.equals(k)))
```

- 遍历完整个链表都没有找到可替代的节点，那么将这个新的`HashMapEntry`作为链表的头节点，并且也是数组在该位置上的元素，原先的头节点则作为它的后继节点。

### 9.7.2 HashMapEntry 的数据结构

`HashMapEntry`的定义如下：

```java
static class HashMapEntry<K,V> implements Map.Entry<K,V> {
        //Key
        final K key;
        //Value
        V value;
        //后继节点。
        HashMapEntry<K,V> next;
        //如果 Key 不为 null ，那么就是它的哈希值，否则为0。
        int hash;
        //....
}
```

### 9.7.3元素写入

在第一小节中，我们简要的计算了`HashMap`的整体结构，由此我们可以推断出在设计的时候应当尽可能地使元素均匀分布，使得数组每个位置上的链表尽可能地短，避免从链表头结点开始遍历的过程。

而元素是否分布均匀就取决于根据`Key`的`Hash`值计算数组下标的过程，首先我们看一下`Hash`值的计算，这里首先调用对象的`hashCode`方法，再通过二次`Hash`算法获得一个`Hash`值：



```java
    public static int secondaryHash(Object key) {
        return secondaryHash(key.hashCode());
    }

    private static int secondaryHash(int h) {
        h += (h <<  15) ^ 0xffffcd7d;
        h ^= (h >>> 10);
        h += (h <<   3);
        h ^= (h >>>  6);
        h += (h <<   2) + (h << 14);
        return h ^ (h >>> 16);
    }
```

之后，再通过这个计算出来`Hash`值 **与上当前数组长度减一** 进行取余，获得对应的数组下标：



```java
hash & (tab.length - 1)
```

由于`HashMap`在扩容的时候，保证了数组的长度适中为`2`的`n`幂，因此`length - 1`的二进制表示始终为全`1`，因此进行`&`操作的结果既保证了最终的结果不会超过数组的长度范围，同时也保证了两个`Hash`值相同的元素不会映射到数组的同一位置，再加上上面二次`Hash`的过程加上了高位的计算优化，从而使得数据的分布尽可能地平均。

### 9.7.4 元素读取

理解了上面存储的过程，读取自然也就很好理解了，其实通过`Key`计算数组下标，遍历该位置上数组元素的链表进行查找的过程。

### 9.7.5 扩容

当`HashMap`中的元素越来越多的时候，`hash`冲突的几率也就越来越高，因为数组的长度是固定的，所以为了提高查询的效率，就要对`HashMap`的数组进行扩容。

当`HashMap`中的元素个数超过数组大小 * `loadFactor`时，`loadFactor`的默认值为0.75，就会进行数组扩容，扩容后的大小为原先的`2`倍，然后重新计算每个元素在数组中的位置，原数组中的数据必须重新计算其在新数组中的位置，并放进去。

扩容是一个相当耗费性能的操作，因此如果我们已经预知HashMap中元素的个数，那么预设元素的个数能够有效的提高`HashMap`的性能。

### 9.7.6 Fail-Fast 机制

`HashMap`并不是线程安全的，因此如果在使用迭代器的过程中有其他线程修改了`map`，那么将抛出`ConcurrentModificationException`，这就是所谓`fail-fast`策略。

这一策略在源码中的实现是通过`modCount`域，`modCount`顾名思义就是修改次数，对`HashMap`内容的修改都将增加这个值，那么在迭代器初始化过程中会将这个值赋给迭代器的`expectedModCount`。

在迭代过程中，判断`modCount`跟`expectedModCount`是否相等，如果不相等就表示已经有其他线程修改了`Map`，那么就会通过下面的方法抛出异常：

```java
HashMapEntry<K, V> nextEntry() {
    if (modCount != expectedModCount)
        throw new ConcurrentModificationException();
    //省略...
}
```

`modCount`声明为`volatile`，保证了多线程情况下的内存可见性。

在迭代器创建之后，如果从结构上对映射进行修改，除非通过迭代器本身的`remove`方法，其他任何时间任何方式的修改，迭代器都将抛出`ConcurrentModificationException`。因此，面对并发的修改，迭代器很快就会完全失败，而不保证在将来不确定的时间发生任意不确定行为的风险



## 9.8 HashTable

`HashTable`经常用来和`HashMap`进行比较，**前者是线程安全的，而后者则不是**，其实`HashTable`要比`HashMap`出现得要早，它实现线程安全的原理并没有什么高级的地方，只不过是在写入和读取时加上了`synchronized`关键字用于同步，并且也**不推荐使用**了，因为在并发包中提供了更好的解决方案`ConcurrentHashMap`，它内部的实现比较复杂，之后我们再通过一篇文章进行分析。

这里简单地总结一下它和`HashMap`之间的区别：

- `HashTable`基于`Dictionary`类，而`HashMap`是基于`AbstractMap`。`Dictionary`是任何可将键映射到相应值的类的抽象父类，而`AbstractMap`基于 `Map`接口的实现，它以最大限度地减少实现此接口所需的工作。
- `HashMap`的`key`和`value`都允许为`null`，而**`Hashtable`的`key`和`value`都不允许为`null`**。`HashMap`遇到`key`为`null`的时候，调用`putForNullKey`方法进行处理，而对`value`没有处理，`Hashtable`遇到`null`，直接返回 `NullPointerException`。
- `Hashtable`方法是同步，而`HashMap`则不是。我们可以看一下源码，`Hashtable`中的几乎所有的`public`的方法都是`synchronized`的，而有些方法也是在内部通过`synchronized`代码块来实现。所以有人一般都建议如果是涉及到多线程同步时采用`HashTable`，没有涉及就采用`HashMap`，但是在 `Collections`类中存在一个静态方法：`synchronizedMap()`，该方法创建了一个线程安全的`Map`对象，并把它作为一个封装的对象来返回。

## 9.9 TreeMap

`TreeMap`是一个有序的`key-value`集合，它是通过 [红黑树](https://link.jianshu.com?t=http://www.cnblogs.com/skywang12345/p/3245399.html) 实现的。`TreeMap`继承于`AbstractMap`，所以它是一个`Map`，即一个`key-value`集合。`TreeMap`实现了`NavigableMap`接口，意味着它支持一系列的导航方法，比如返回有序的key集合。`TreeMap`实现了`Cloneable`和`Serializable`接口，意味着它可以被`Clone`和序列化。

`TreeMap`基于红黑树实现，该映射根据其键的自然顺序进行排序，或者根据创建映射时提供的 `Comparator`进行排序，具体取决于使用的构造方法。`TreeMap`的基本操作`containsKey`、`get`、`put`和`remove`的时间复杂度是`log(n)` ，另外，`TreeMap`是非同步的， 它的`iterator`方法返回的迭代器是`Fail-Fastl`的。

## 9.10 LinkedHashMap

- `LinkedHashMap`是`HashMap`的子类，与`HashMap`有着同样的存储结构，但它加入了一个双向链表的头结点，将所有`put`到`LinkedHashmap`的节点一一串成了一个双向循环链表，因此它保留了节点插入的顺序，可以使节点的输出顺序与输入顺序相同。
- `LinkedHashMap`可以用来实现`LRU`算法。
- `LinkedHashMap`同样是非线程安全的，只在单线程环境下使用。

## 9.11 LinkedHashSet

`LinkedHashSet`是具有可预知迭代顺序的`Set`接口的哈希表和链接列表实现。此实现与`HashSet`的不同之处在于，后者维护着一个运行于所有条目的双重链接列表。此链接列表定义了迭代顺序，该迭代顺序可为插入顺序或是访问顺序。

`LinkedHashSet`的实现：对于`LinkedHashSet`而言，它继承与`HashSet`、又基于`LinkedHashMap`来实现的。



# 十、内存区域

## 10.1 概述

`Java`虚拟机在执行`Java`程序的过程中会把它所**管理的内存**划分为若干个不同的区域，它们有的随着**虚拟机进程**的启动而存在，有些区域则依赖**用户线程**的启动和结束而建立而销毁。
 下面，我们就分两个部分讨论：

- 线程隔离的数据区
- 所有线程共享的数据区

## 10.2 线程隔离的数据区

### 10.2.1 程序计数器（没有OOM情况）

- 概念
   程序计数器是**当前线程所执行的字节码的行号指示器**。字节码解释器工作时会通过改变这个计数器的指向来取下一条需要执行的字节码指令。
   如果线程正在执行的是`Java`方法，那么计数器记录的是**正在执行的虚拟机字节码指令的地址**；如果正在执行的是`Native`方法，那**这个计数器则为空**。
   此内存区域是唯一一个在`Java`虚拟机规范中没有规定任何`OOM`（java.lang.OutOfMemoryError）情况的区域。
- 为什么需要线程隔离
   由于`Java`虚拟机的多线程是通过线程轮流切换并分配处理器执行时间的方式来实现的，因此，为了线程切换后能恢复到正确的执行位置，**每条线程都需要有一个独立的程序计数器**。

### 10.2.2 `Java`虚拟机栈

- 概念
   虚拟机栈描述的是`Java`方法执行的内存模型，每个方法在执行的同时会创建一个栈帧，用于存储**局部变量表、操作数栈、动态链接、方法出口**等信息。
   **局部变量表**：存放了编译期可知的各种基本数据类型（`boolean/byte/..`），对象引用（指向对象起始地址的引用指针，或者是指向一个代表对象的句柄，或者是其它与此对象相关的位置）和`returnAddress`地址。
   局部变量表**所需的内存空间在编译期间完成分配**，当进入一个方法时，这个方法需要在栈中分配多大局部变量空间是完全确定的，在方法运行期间不会改变局部变量表的大小。
   每一个方法从调用到执行完成的过程，就对应一个栈帧在虚拟机栈中入栈到出栈的过程。
- 为什么需要线程隔离
   因为**每个线程所执行的逻辑和时序不同**，所以它们的虚拟机栈自然也就不会一定相同，因此不能共用。
- 异常
   如果线程请求的栈深度大于虚拟机所允许的深度，会抛出`StackOverflowError`异常；如果虚拟机栈可以动态扩展，当扩展时无法申请到足够的内存，就会抛出`OOM`。

### 10.2.3 本地方法栈

- 概念
   和`Java`虚拟机方法栈类似，不过本地方法栈为虚拟机使用到的`Native`方法，有些虚拟机（譬如`HotSpot`）直接将本地方法栈和虚拟栈合二为一。
- 异常
   和虚拟机栈相同。

## 10.3线程共享的数据区

### 10.3.1 `Java`堆

- 概念
   **`Java`堆在虚拟机启动时创建，它的目的是存放对象实例，它也是垃圾收集器管理的主要区域。**
   `Java`堆**可以处于物理上不连续的内存空间中，只要逻辑上连续即可**，在实现上，既可以实现成固定大小的，也可以是可扩展的。
- 异常
   如果在堆中没有内存完成实例分配，并且堆也无法再扩展时，会抛出`OOM`异常。

下面我们讨论一下堆中的**对象分配、布局和访问过程**：

#### 10.3.1.1 对象的创建

对象的创建分为以下几步：

- 第一步：当虚拟机遇到一条`new`指令时，首先将去检查这个指令的参数是否能在常量池中定位到一个类的符号引用，并且检查这个符号引用代表的类是否已经被加载、解析和初始化过，如果没有，那么必须执行相应的类加载过程，
- 第二步：接下来虚拟机将为新生对象分配内存，对象所需内存的大小在类加载完成后便可确定，分配的方式有两种：
  - 指针碰撞：用过和空闲的内存以指针作为分界点的指示器，分配内存就是把指针向空闲空间那边挪动一个与对象大小相等距离，这种方式要求内存是规整的。
  - 空闲列表：维护一个列表，记录哪些内存是可用的，在分配和回收时更新列表。
  - 对分配内存空间的动作进行同步，虚拟机上采用`CAS`配上失败重试的方式保证更新操作的原子性。
  - 每个线程在`Java`堆中预先分配一小块内存，成为本地线程分配缓冲`TLAB`，哪个线程需要分配内存，就在哪个线程的`TLAB`上分配，只有`TLAB`用完需要分配新的`TLAB`才需要同步。
- 第三步：在内存分配完成，把除了对象头之外的分配到的内存空间都初始化为零值，接下来就是对对象进行必要的设置，这些信息存放在对象头中。
- 第四步：当对象头设置完毕之后，从虚拟机的视角来看，新的对象就产生了，接着就执行`<init>`方法，把对象按照程序员的意愿进行初始化。

#### 10.3.1.2 对象的内存布局

对象在内存中存储的布局可以分为三个区域：对象头、实例数据、对其填充。

- 对象头
  - 存储对象自身的运行时数据：`HashCode`、`Gc`分代年龄、锁状态标志、线程持有的锁、偏向线程`ID`、偏向时间戳等，所占位数和虚拟机位数相同。
  - 类型指针：对象指向它的**类元数据**的指针，虚拟机通过这个指针来确定这个对象是哪个类的实例
  - 如果对象是一个`Java`数组，那么在对象头中还必须有一块记录数组长度的数据。
- 实例数据
   包括在父类和子类中所定义的各种类型的字段内容，存储顺序收到虚拟机分配策略参数的影响，相同宽度的字段总是被分配到一起，在满足这个前提条件下，父类中定义的变量会出现在子类之前。
- 对齐填充
   `HotSpot`要求对象的大小必须是`8`字节的整数倍，而对象头部分正好是`8`字节的整数倍，当对象实例数据部分没有对齐时，就需要通过对齐填充来不全。

#### 10.3.1.3 对象的访问

对象的访问有两种方式：

- 使用句柄，`Java`堆中划分出一块内存作为句柄池，`reference`中存储的就是对象的句柄地址，而句柄中包含了对象实例数据与类型数据各自的地址信息。
   优点：`reference`中存储的是稳定的句柄地址，在对象被移动时，只会改变句柄中的实例数据指针，而`reference`本身不需要修改。
- 直接指针：在`Java`堆对象的布局中放置访问类型数据的相关信息，`reference`中存储的直接就是对象地址。
   优点：速度快，`HotSpot`采用的就是这种方式。

### 10.3.2 方法区

- 概念
   方法区用于存储已被虚拟机加载的**类信息、常量、静态变量**、即时编译器编译后的代码等数据。
- 为什么把方法区称为“永久代”
   `HotSpot`选择把`GC`分代收集器扩展至方法区，或者说用永久代来实现方法区，这样`HotSpot`的垃圾收集器可以像管理`Java`堆一样管理这部分内存，能够省去专门为这个方法区编写内存管理的代码。
   对这区域的内存回收主要是针对常量池的回收和对类型的卸载。
- 异常
   当方法区无法满足内存分配需求时，将抛出`OOM`。
- 运行时常量池
   运行时常量池是方法区的一部分。
   `Class`文件中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是常量池，用于存放编译期生成的各种字面量和符号引用，这部分内容将在类加载后进入方法区的运行时常量池中存放。
   并非预置入`Class`文件中常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中，例如`String`类的`intern`方法。
   当常量池中无法再申请内存，就会抛出`OOM`异常。

[常量池](https://blog.csdn.net/qq_26222859/article/details/73135660)

1. 局部变量表：存放方法中的参数和方法内部定义的局部变量。
2. 操作数栈：存取方法执行过程中字节码指令操作的内容，算数操作和参数的传递都是在操作栈中进行的。
3. 动态链接：保留常量池中的符号引用，在真正方法调用时会以常量池中的符号引用来获取直接引用。
4. 方法出口：一般情况下保留调用者的程序计数器值，在方法异常退出情况下，返回地址通过异常表来处理。



# 十一、垃圾回收

## 11.1 概述

`GC`需要考虑的三个问题：

- 哪些内存需要回收
- 什么时候回收
- 如何回收

在分析内存区域的时候，我们把`Java`运行时数据区分为两个部分：

- 程序计数器、虚拟机栈、本地方法栈：每个栈帧中分配多少内存在类结构确定下来就已知，因此这些区域的内存分配和回收具备确定性，方法结束或线程结束时，内存就跟着被回收了。
- `Java`堆、方法区：由于一个接口中的多个实现类需要的内存可能不一样，一个方法中的多个分支需要的内存也不一样，只有在程序处于运行期间才能知道会创建哪些对象，因此这些区域的内存分配和回收是动态的。

## 11.2 如何判断哪些是“存活”的实例

### 11.2.1 引用的分类

引用的定义：如果`reference`类型的数据中**存储的数值代表的另外一块内存的起始地址**，就称这块内存代表引用。
 引用的分类：

- 强引用（`Object a = new Object()`）：只要强引用存在，垃圾回收器永远不会回收掉被引用的对象。
- 软引用（`SoftReference`）：有用但并非必须，在系统将要发生`OOM`异常之前，将会把这些对象列进回收范围中进行第二次回收。
- 弱引用（`WeakReference`）：非必须对象，被弱引用的对象只能生存到下一次垃圾收集发生前。
- 虚引用（`PhantomReference`）：不会对生存时间产生影响，也无法通过虚引用来取得一个对象实例，设置虚引用的唯一目的就是能在这个对象被垃圾回收器回收时收到一个系统通知。

### 11.2.2 引用计数法

给对象添加一个引用计数器，当有一个地方引用它时就加一，引用失效时就减一，当计数器的值为零时表示它不可用。
 但是它无法解决**相互循环引用问题**。

### 11.2.3 可达性分析

通过一系列的称为`GC Roots`的对象作为起始点，从这些节点开始向下搜索，所走过的路径称为**引用链**。当一个对象到`GC Roots`没有任何引用链时，表示这个对象不可用，`GC Roots`的类型有：

- 虚拟机栈中的**局部变量表**中引用的对象。
- 方法区中**类静态属性**引用的对象。
- 方法区中**常量**引用的对象。
- 本地方法栈中**`JNI`**引用的对象。

### 11.2.4 `finalize`方法对于内存回收的影响

当某个对象在经过可达性分析后，发现它到`GC Roots`没有任何引用链时，那么它会被第一次标记，并进行第一次筛选，筛选的结果有两种情况：

- 没有覆盖`finalize()`方法或者虚拟机已经调用过它的`finalize()`方法：直接回收。
- 其它情况：把这个对象放置在一个`F-Queue`的队列中，并在稍后由一个由虚拟机自动建立的、低优先级的`Finalizer`线程去执行这个对象的`finalize()`方法，如对象要在`finalize`方法中拯救自己，只要重新与引用链的某个变量关联即可，那么在第二次标记时它将被移出“即将回收”的集合，否则它将被回收。

这种方法代价高昂，不确定性大，无法保证各个对象的调用顺序，因此可以忘记这个方法的存在。

## 11.3 方法区的回收

对于方法区（`HotSpot`中的永久代）主要回收两部分内容：废弃常量和无用的类。

- 废弃常量
   以常量池中字面量的回收为例，如果一个字符串`abc`被放入了常量池中，但是没有任何一个`String`对象引用它，那么就会被清理出常量池，常量池中其它类（接口）、方法、字段的符号引用也类似。
- 类，同时满足三个条件：
  - 该类的所有实例已经被回收
  - 加载该类的`ClassLoader`已经被回收
  - 该类对应的`java.lang.Class`对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法。

# 11.4 垃圾收集算法基础

### 11.4.1 标记 - 清除算法

- 概念
   首先标记出所有需要回收的对象，在标记完成后统一进行回收。
- 缺点：
  - 标记和清除两个过程效率不高。
  - 产生内存碎片，导致需要分配较大对象时，无法找到足够的连续内存而需要触发一次`GC`操作。

### 11.4.2 复制算法

- 概念
   将可用内存划分为大小相等的两块，每次只使用其中的一块，当一块内存用完了。则触发一次`GC`操作，将活着的对象复制到另一块上，然后再把已使用的内存空间一次清理掉。
- 缺点
   将内存缩小为了原来的一半。
- 现在商业虚拟机采用这种算法的改良版来实现**新生代的回收**
   它把内存按`8:1:1`分为`Eden/survivor0/survivor1`三块：
   需要分配内存时，首先尝试在`Eden`区分配，如果`Eden`区无法分配，那么尝试把活着的对象放到`survivor0`中去：
- 如果`survivor0`可以放入，那么放入之后清除`Eden`区。
- 如果`survivor0`不可以放入，那么尝试把`Eden`和`survivor0`的存活对象放到`survivor1`中：
  - 如果`survivor1`可以放入，那么放入`survivor1`之后清除`Eden`和`survivor0`，之后再把`survivor1`中的对象复制到`survivor0`中，保持`survivor1`一直为空。
  - 如果`survivor1`不可以放入，那么直接把它们放入到老年代中，并清除`Eden`和`survivor0`，这个过程也称为**分配担保**。
- 适用情况
   由于复制算法在**对象成活率**较高时，需要较多的复制操作，效率会变低，所以在老年代中不能采用该算法。

### 11.4.3 标记 - 整理算法

- 概念
   和标记 - 清除算法类似，但后续步骤不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动，然后直接清除掉端边界以外的内存。
- 优点
   解决了**标记- 清除算法导致的内存碎片问题**和**在存活率较高时复制算法效率低的问题**。

### 11.4.4 分代收集算法

当前商业虚拟机采用的方式，根据对象存活周期的不同将内存划分为几块，一般是新生代和老年代：

- 新生代：每次垃圾收集时只有少量存活，选用复制算法的改良版，也就是上面说到的`Eden/survivor0/survivor1`的分配方式。
- 老年代：对象存活率较高，且没有分配担保，必须用标记 - 清除或标记 - 整理算法来实现。

## 11.5 `Minor GC`和`Major GC/Full GC`

- `Minor GC`：发生在新生代的垃圾回收动作，非常频繁，回收速度也较快，采用的垃圾收集器有`Serial`、`ParNew`、`Parallel Scavenge`。
- `Major GC/Full GC`：发生在老年代的`GC`，经常伴随至少一次的`Minor GC`，`Major GC`的速度一般会比`Minor GC`慢十倍以上，采用的垃圾收集器有`CMS`、`Serial Old`、`Parallel Old`。

## 11.6 对象分配的原则

- 对象优先在`Eden`区分配
   当`Eden`区没有足够空间，触发一次`Minor GC`。
- 大对象直接进入老年代
   例如很长的字符串以及数组，经常出现大对象容易导致内存还有不少空间时就提前触发垃圾收集以获取足够的连续空间来“安置”它们。
- 长期存活的对象将进入老年代
   如果`Eden`区出生并进过第一次`Minor GC`后，仍然存活，并且被成功复制到`survivor`区中，那么对象年龄变为一，当对象在`survivor`中每熬过一次`Minor GC`，年龄就增加一，当年龄增加到一定程度，就会晋升到老年代中。
- 动态对象年龄绑定
   如果`survivor`空间中相同年龄所有对象大小的总和大于`survivor`空间的一半，年龄大于或等于该年龄的对象就可以进入老年代，无须到达要求的年龄。
- 空间分配担保
   在发生`Minor GC`前，检查老年代最大可用连续空间是否大于新生代所有对象总空间：
- 大于，那么操作是安全的，不对老年代进行`Full GC`。
- 小于，检查`HandlePromotionFailure`设置值是否允许失败：
  - 允许：检查老年代最大可用连续空间是否大于历次晋升到老年代对象的平均大小：
    - 大于：不对老年代进行`Full GC`。在这之后，因为有可能出现某次存活对象激增的情况，这种属于冒险行为，如果出现了担保失败（也就是`Eden`和`survivor0`的存活对象既无法放入`survivor1`，也无法放入老年代的连续空间中），那么会在失败之后对老年代进行`Full GC`。
    - 小于：先对老年代进行一次`Full GC`。
  - 不允许：先对老年代执行一次`Full GC`。



# 十二、类加载

# 十三、泛型

# 十四、反射

