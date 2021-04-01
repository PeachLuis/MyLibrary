# 1. String使用+连接的三种比较情况

```java
//情况1：
String a1 = "a" + "b";
String a2 = "ab";
System.out.println(a1 == a2);   //true
//情况2：
String b1 = "a";
String b2 = b1 + "b";
String b3 = "ab";
System.out.println(b2 == b3);   //false
//情况3：
String c1 = "a";
String c2 = "b";
String c3 = c1 + c2;
String c4 = "ab";
System.out.println(c3 == c4);   //false
```

- 情况1在编译时便合成常量ab，所以都指向String常量池中相同地址；
- 情况2和3都是在连接的时候会调用StringBuilder函数，然后通过append方法来添加，最后会在**堆**中创建一个新的String对象，由于堆中不同对象地址不同，所以返回false，但其值value存放的是String常量池中的地址，是相同的。



# 2. Java语法糖



# 3. Java集合框架中的设计模式



# 4. System.arraycopy是深拷贝还是浅拷贝

```java
public class Test {

    public static void main(String[] args) {
        //针对基础数据类型
        int[] a = {1, 2, 3, 4, 5};
        int[] b = new int[a.length];
        System.arraycopy(a, 0, b, 0, a.length);
        a[0] = 5;
        System.out.println(Arrays.toString(a));
        System.out.println(Arrays.toString(b));


        System.out.println("********");
        //针对引用类型
        People[] ps1 = {
                new People("小王", 20),
                new People("小刘", 21)
        };
        People[] ps2 = new People[ps1.length];
        System.arraycopy(ps1, 0, ps2, 0, ps1.length);
        ps1[0].age = 19;
        System.out.println(Arrays.toString(ps1));
        System.out.println(Arrays.toString(ps2));
    }
}

class People {
    String name;

    int age;

    public People(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "People{" +
                "name='" + name +
                ", age=" + age +
                '}';
    }
}
```

输出：

```
[5, 2, 3, 4, 5]
[1, 2, 3, 4, 5]
********
[People{name='小王, age=19}, People{name='小刘, age=21}]
[People{name='小王, age=19}, People{name='小刘, age=21}]
```

所以得到结论：

- 针对基础数据类型，是深拷贝
- 针对引用类型，只是对引用的拷贝，但是值不是深拷贝