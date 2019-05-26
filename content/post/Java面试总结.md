---
title: Java面试题整理
date: 2018-06-25 21:45:35
categories: Java
tags: ["面试"]
photos:
 - https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/bing_photos/20180628.jpg
---

<p class="description">Java面试题整理,分为基础，框架，算法，数据库四个部分</p>
<!-- more -->


## <div class="note default"><p>基础篇</p></div>

### 1.面向对象的特征有哪些方面？
答：面向对象的特征主要有以下几个方面：
① 抽象：抽象是将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象两方面。抽象只关注对象有哪些属性和行为，并不关注这些行为的细节是什么。
- 继承：继承是从已有类得到继承信息创建新类的过程。提供继承信息的类被称为父类（超类、基类）；得到继承信息的类被称为子类（派生类）。继承让变化中的软件系统有了一定的延续性，同时继承也是封装程序中可变因素的重要手段（如果不能理解请阅读阎宏博士的《Java与模式》或《设计模式精解》中关于桥梁模式的部分）。
② 封装：通常认为封装是把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。面向对象的本质就是将现实世界描绘成一系列完全自治、封闭的对象。我们在类中编写的方法就是对实现细节的一种封装；我们编写一个类就是对数据和数据操作的封装。可以说，封装就是隐藏一切可隐藏的东西，只向外界提供最简单的编程接口（可以想想普通洗衣机和全自动洗衣机的差别，明显全自动洗衣机封装更好因此操作起来更简单；我们现在使用的智能手机也是封装得足够好的，因为几个按键就搞定了所有的事情）。
③ 多态性：多态性是指允许不同子类型的对象对同一消息作出不同的响应。简单的说就是用同样的对象引用调用同样的方法但是做了不同的事情。多态性分为编译时的多态性和运行时的多态性。如果将对象的方法视为对象向外界提供的服务，那么运行时的多态性可以解释为：当A系统访问B系统提供的服务时，B系统有多种提供服务的方式，但一切对A系统来说都是透明的（就像电动剃须刀是A系统，它的供电系统是B系统，B系统可以使用电池供电或者用交流电，甚至还有可能是太阳能，A系统只会通过B类对象调用供电的方法，但并不知道供电系统的底层实现是什么，究竟通过何种方式获得了动力）。方法重载（overload）实现的是编译时的多态性（也称为前绑定），而方法重写（override）实现的是运行时的多态性（也称为后绑定）。运行时的多态是面向对象最精髓的东西，要实现多态需要做两件事：1). 方法重写（子类继承父类并重写父类中已有的或抽象的方法）；2). 对象造型（用父类型引用引用子类型对象，这样同样的引用调用同样的方法就会根据子类对象的不同而表现出不同的行为）。
<br/>
### 2.访问修饰符public,private,protected,以及不写（默认）时的区别？

修饰符 | 当前类 | 同包 | 子类 | 其他包 
-| :-: | :-: | :-:| :-:
public |√|√|√|√
protected |√|√|√|×
default |√|√|×|×
private |√|×|×|×

类的成员不写访问修饰时默认为default。默认对于同一个包中的其他类相当于公开（public），对于不是同一个包中的其他类相当于私有（private）。受保护（protected）对子类相当于公开，对不是同一包中的没有父子关系的类相当于私有。Java中，外部类的修饰符只能是public或默认，类的成员（包括内部类）的修饰符可以是以上四种。
<br/>
### 3.String 是最基本的数据类型吗？
不是。Java中的基本数据类型只有8个：byte、short、int、long、float、double、char、boolean；除了基本类型（primitive type）和枚举类型（enumeration type），剩下的都是引用类型（reference type）。
<br/>
### 4.float f=3.4;是否正确？
答:不正确。3.4是双精度数，将双精度型（double）赋值给浮点型（float）属于下转型（down-casting，也称为窄化）会造成精度损失，因此需要强制类型转换float f =(float)3.4; 或者写成float f =3.4F;。
<br/>
### 5.hort s1 = 1; s1 = s1 + 1;有错吗?short s1 = 1; s1 += 1;有错吗？
对于short s1 = 1; s1 = s1 + 1;由于1是int类型，因此s1+1运算结果也是int 型，需要强制转换类型才能赋值给short型。而short s1 = 1; s1 += 1;可以正确编译，因为s1+= 1;相当于s1 = (short)(s1 + 1);其中有隐含的强制类型转换。
<br/>
### 6.int和Integer有什么区别？
答：Java是一个近乎纯洁的面向对象编程语言，但是为了编程的方便还是引入了基本数据类型，但是为了能够将这些基本数据类型当成对象操作，Java为每一个基本数据类型都引入了对应的包装类型（wrapper class），int的包装类就是Integer，从Java 5开始引入了自动装箱/拆箱机制，使得二者可以相互转换。
Java 为每个原始类型提供了包装类型：
① 原始类型: boolean，char，byte，short，int，long，float，double
② 包装类型：Boolean，Character，Byte，Short，Integer，Long，Float，Double
Example:
``` java
class AutoUnboxingTest {
 
    public static void main(String[] args) {
        Integer a = new Integer(3);
        Integer b = 3;                  // 将3自动装箱成Integer类型
        int c = 3;
        System.out.println(a == b);     // false 两个引用没有引用同一对象
        System.out.println(a == c);     // true a自动拆箱成int类型再和c比较
    }
}
```
陷阱:
``` java
public class Test03 {
 
    public static void main(String[] args) {
        Integer f1 = 100, f2 = 100, f3 = 150, f4 = 150;
 
        System.out.println(f1 == f2);
        System.out.println(f3 == f4);
    }
}
```
如果整型字面量的值在-128到127之间，那么不会new新的Integer对象，而是直接引用常量池中的Integer对象，所以上面的面试题中f1==f2的结果是true，而f3==f4的结果是false。
<br/>
### 7.&和&&的区别？
答：&运算符有两种用法：(1)按位与；(2)逻辑与。&&运算符是短路与运算。逻辑与跟短路与的差别是非常巨大的，虽然二者都要求运算符左右两端的布尔值都是true整个表达式的值才是true。&&之所以称为短路运算是因为，如果&&左边的表达式的值是false，右边的表达式会被直接短路掉，不会进行运算。很多时候我们可能都需要用&&而不是&，例如在验证用户登录时判定用户名不是null而且不是空字符串，应当写为：username != null &&!username.equals(“”)，二者的顺序不能交换，更不能用&运算符，因为第一个条件如果不成立，根本不能进行字符串的equals比较，否则会产生NullPointerException异常。注意：逻辑或运算符（|）和短路或运算符（||）的差别也是如此。
<br/>

### 8.解释内存中的栈(stack)、堆(heap)和静态区(static area)的用法。
答：通常我们定义一个基本数据类型的变量，一个对象的引用，还有就是函数调用的现场保存都使用内存中的栈空间；而通过new关键字和构造器创建的对象放在堆空间；程序中的字面量（literal）如直接书写的100、”hello”和常量都是放在静态区中。栈空间操作起来最快但是栈很小，通常大量的对象都是放在堆空间，理论上整个内存没有被其他进程使用的空间甚至硬盘上的虚拟内存都可以被当成堆空间来使用。
Example：
``` java
String str = new String("hello");
```
上面的语句中变量str放在栈上，用new创建出来的字符串对象放在堆上，而”hello”这个字面量放在静态区。
<div class="note warning"><p> 补充：较新版本的Java（从Java6的某个更新开始）中使用了一项叫”逃逸分析”的技术，可以将一些局部对象放在栈上以提升对象的操作性能。</p></div>

### 9.switch 是否能作用在byte 上，是否能作用在long 上，是否能作用在String上？
答：在Java 5以前，switch(expr)中，expr只能是byte、short、char、int。从Java 5开始，Java中引入了枚举类型，expr也可以是enum类型，从Java 7开始，expr还可以是字符串（String），但是长整型（long）在目前所有的版本中都是不可以的。
<br/>

### 10.在Java中，如何跳出当前的多重嵌套循环？
答：在最外层循环前加一个标记如A，然后用break A;可以跳出多重循环。（Java中支持带标签的break和continue语句，作用有点类似于C和C++中的goto语句，但是就像要避免使用goto一样，应该避免使用带标签的break和continue，因为它不会让你的程序变得更优雅，很多时候甚至有相反的作用，所以这种语法其实不知道更好）
<br/>

### 11.构造器（constructor）是否可被重写（override）？
答：构造器不能被继承，因此不能被重写，但可以被重载。
<br/>

### 12.两个对象值相同(x.equals(y) == true)，但却可有不同的hash code，这句话对不对
答：不对，如果两个对象x和y满足x.equals(y) == true，它们的哈希码（hash code）应当相同。Java对于eqauls方法和hashCode方法是这样规定的：(1)如果两个对象相同（equals方法返回true），那么它们的hashCode值一定要相同；(2)如果两个对象的hashCode相同，它们并不一定相同。当然，你未必要按照要求去做，但是如果你违背了上述原则就会发现在使用容器时，相同的对象可以出现在Set集合中，同时增加新元素的效率会大大下降（对于使用哈希存储的系统，如果哈希码频繁的冲突将会造成存取性能急剧下降）。

<div class="note warning"><p> 补充： 先equals方法必须满足自反性（x.equals(x)必须返回true）、对称性（x.equals(y)返回true时，y.equals(x)也必须返回true）、传递性（x.equals(y)和y.equals(z)都返回true时，x.equals(z)也必须返回true）和一致性（当x和y引用的对象信息没有被修改时，多次调用x.equals(y)应该得到同样的返回值），而且对于任何非null值的引用x，x.equals(null)必须返回false。实现高质量的equals方法的诀窍包括：1. 使用==操作符检查”参数是否为这个对象的引用”；2. 使用instanceof操作符检查”参数是否为正确的类型”；3. 对于类中的关键属性，检查参数传入对象的属性是否与之相匹配；4. 编写完equals方法后，问自己它是否满足对称性、传递性、一致性；5. 重写equals时总是要重写hashCode；6.不要将equals方法参数中的Object对象替换为其他的类型，在重写时不要忘掉@Override注解。</p></div>

### 13.是否可以继承String类？
答：String 类是final类，不可以被继承。

<div class="note warning"><p> 补充：继承String本身就是一个错误的行为，对String类型最好的重用方式是关联关系（Has-A）和依赖关系（Use-A）而不是继承关系（Is-A）。</p></div>

### 14.当一个对象被当作参数传递到一个方法后，此方法可改变这个对象的属性，并可返回变化后的结果，那么这里到底是值传递还是引用传递？
答：是值传递。Java语言的方法调用只支持参数的值传递。当一个对象实例作为一个参数被传递到方法中时，参数的值就是对该对象的引用。对象的属性可以在被调用过程中被改变，但对对象引用的改变是不会影响到调用者的。C++和C#中可以通过传引用或传输出参数来改变传入的参数的值。在C#中可以编写如下所示的代码，但是在Java中却做不到。

<div class="note info"><p>说明：Java中没有传引用实在是非常的不方便，这一点在Java 8中仍然没有得到改进，正是如此在Java编写的代码中才会出现大量的Wrapper类（将需要通过方法调用修改的引用置于一个Wrapper类中，再将Wrapper对象传入方法），这样的做法只会让代码变得臃肿，尤其是让从C和C++转型为Java程序员的开发者无法容忍。</p></div>

### 15.String和StringBuilder、StringBuffer的区别？
答：Java平台提供了两种类型的字符串：String和StringBuffer/StringBuilder，它们可以储存和操作字符串。其中String是只读字符串，也就意味着String引用的字符串内容是不能被改变的。而StringBuffer/StringBuilder类表示的字符串对象可以直接进行修改。StringBuilder是Java 5中引入的，它和StringBuffer的方法完全相同，区别在于它是在单线程环境下使用的，因为它的所有方面都没有被synchronized修饰，因此它的效率也比StringBuffer要高。
Example:
``` java
class StringEqualTest {
    public static void main(String[] args) {
        String s1 = "Programming";
        String s2 = new String("Programming");
        String s3 = "Program" + new String("ming");
        String s4 = "Program" + "ming";
        System.out.println(s1 == s2);
        System.out.println(s1 == s3);
        System.out.println(s1 == s4);
        System.out.println(s1 == s1.intern());
    }
}
结果: false 
      false
      true
      true
```
<div class="note warning">补充：String对象的intern方法会得到字符串对象在常量池中对应的版本的引用（如果常量池中有一个字符串与String对象的equals结果是true），如果常量池中没有对应的字符串，则该字符串将被添加到常量池中，然后返回常量池中字符串的引用。</p></div>

### 16.重载（Overload）和重写（Override）的区别。重载的方法能否根据返回类型进行区分？
答：方法的重载和重写都是实现多态的方式，区别在于前者实现的是编译时的多态性，而后者实现的是运行时的多态性。重载发生在一个类中，同名的方法如果有不同的参数列表（参数类型不同、参数个数不同或者二者都不同）则视为重载；重写发生在子类与父类之间，重写要求子类被重写方法与父类被重写方法有相同的返回类型，比父类被重写方法更好访问，不能比父类被重写方法声明更多的异常（<font size=4 color= red>里氏代换原则</font>）。重载对返回类型没有特殊的要求。
Example:
面试题:为什么不能根据返回类型来区分重载：函数的返回值只是作为函数运行之后的一个“状态”，他是保持方法的调用者与被调用者进行通信的关键。并不能作为某个方法的“标识”

### 17.<font size=4 color= red>描述一下JVM加载class文件的原理机制？</font>
答：JVM中类的装载是由类加载器（ClassLoader）和它的子类来实现的，Java中的类加载器是一个重要的Java运行时系统组件，它负责在运行时查找和装入类文件中的类。
由于Java的跨平台性，经过编译的Java源程序并不是一个可执行程序，而是一个或多个类文件。当Java程序需要使用某个类时，JVM会确保这个类已经被加载、连接（验证、准备和解析）和初始化。类的加载是指把类的.class文件中的数据读入到内存中，通常是创建一个字节数组读入.class文件，然后产生与所加载类对应的Class对象。加载完成后，Class对象还不完整，所以此时的类还不可用。当类被加载后就进入连接阶段，这一阶段包括验证、准备（为静态变量分配内存并设置默认的初始值）和解析（将符号引用替换为直接引用）三个步骤。最后JVM对类进行初始化，包括：1)如果类存在直接的父类并且这个类还没有被初始化，那么就先初始化父类；2)如果类中存在初始化语句，就依次执行这些初始化语句。
类的加载是由类加载器完成的，类加载器包括：根加载器（BootStrap）、扩展加载器（Extension）、系统加载器（System）和用户自定义类加载器（java.lang.ClassLoader的子类）。从Java 2（JDK 1.2）开始，类加载过程采取了父亲委托机制（PDM）。PDM更好的保证了Java平台的安全性，在该机制中，JVM自带的Bootstrap是根加载器，其他的加载器都有且仅有一个父类加载器。类的加载首先请求父类加载器加载，父类加载器无能为力时才由其子类加载器自行加载。JVM不会向Java程序提供对Bootstrap的引用。下面是关于几个类加载器的说明：
①Bootstrap：一般用本地代码实现，负责加载JVM基础核心类库（rt.jar）；
②Extension：从java.ext.dirs系统属性所指定的目录中加载类库，它的父加载器是Bootstrap；
③System：又叫应用类加载器，其父类是Extension。它是应用最广泛的类加载器。它从环境变量classpath或者系统属性java.class.path所指定的目录中记载类，是用户自定义加载器的默认父加载器。
<br/>
### 18.char 型变量中能不能存贮一个中文汉字，为什么？
答：char类型可以存储一个中文汉字，因为Java中使用的编码是Unicode（不选择任何特定的编码，直接使用字符在字符集中的编号，这是统一的唯一方法），一个char类型占2个字节（16比特），所以放一个中文是没问题的。
<div class="note warning"><p> 补充：使用Unicode意味着字符在JVM内部和外部有不同的表现形式，在JVM内部都是Unicode，当这个字符被从JVM内部转移到外部时（例如存入文件系统中），需要进行编码转换。所以Java中有字节流和字符流，以及在字符流和字节流之间进行转换的转换流，如InputStreamReader和OutputStreamReader，这两个类是字节流和字符流之间的适配器类，承担了编码转换的任务；对于C程序员来说，要完成这样的编码转换恐怕要依赖于union（联合体/共用体）共享内存的特征来实现了。</p></div>

### 19.抽象类（abstract class）和接口（interface）有什么异同？
答：抽象类和接口都不能够实例化，但可以定义抽象类和接口类型的引用。一个类如果继承了某个抽象类或者实现了某个接口都需要对其中的抽象方法全部进行实现，否则该类仍然需要被声明为抽象类。接口比抽象类更加抽象，因为抽象类中可以定义构造器，可以有抽象方法和具体方法，而接口中不能定义构造器而且其中的方法全部都是抽象方法。抽象类中的成员可以是private、默认、protected、public的，而接口中的成员全都是public的。抽象类中可以定义成员变量，而接口中定义的成员变量实际上都是常量。有抽象方法的类必须被声明为抽象类，而抽象类未必要有抽象方法。
<br/>
### 20.静态嵌套类(Static Nested Class)和内部类（Inner Class）的不同？
Static Nested Class是被声明为静态（static）的内部类，它可以不依赖于外部类实例被实例化。而通常的内部类需要在外部类实例化后才能实例化。
注意：Java中非静态内部类对象的创建要依赖其外部类对象，上面的面试题中foo和main方法都是静态方法，静态方法中没有this，也就是说没有所谓的外部类对象，因此无法创建内部类对象，如果要在静态方法中创建内部类对象，可以这样做：
Example:
``` java
class Outer {
 
    class Inner {}
 
    public static void foo() { new Inner(); }
 
    public void bar() { new Inner(); }
 
    public static void main(String[] args) {
        new Inner();
    }
}

new Outer().new Inner();
```
<br/>
### 21. Java 中会存在内存泄漏吗，请简单描述。
答：理论上Java因为有垃圾回收机制（GC）不会存在内存泄露问题（这也是Java被广泛使用于服务器端编程的一个重要原因）；然而在实际开发中，可能会存在无用但可达的对象，这些对象不能被GC回收，因此也会导致内存泄露的发生。例如hibernate的Session（一级缓存）中的对象属于持久态，垃圾回收器是不会回收这些对象的，然而这些对象中可能存在无用的垃圾对象，如果不及时关闭（close）或清空（flush）一级缓存就可能导致内存泄露。下面例子中的代码也会导致内存泄露。
``` java
import java.util.Arrays;
import java.util.EmptyStackException;
 
public class MyStack<T> {
    private T[] elements;
    private int size = 0;
 
    private static final int INIT_CAPACITY = 16;
 
    public MyStack() {
        elements = (T[]) new Object[INIT_CAPACITY];
    }
 
    public void push(T elem) {
        ensureCapacity();
        elements[size++] = elem;
    }
 
    public T pop() {
        if(size == 0) 
            throw new EmptyStackException();
        return elements[--size];
    }
 
    private void ensureCapacity() {
        if(elements.length == size) {
            elements = Arrays.copyOf(elements, 2 * size + 1);
        }
    }
}
```
面的代码实现了一个栈（先进后出（FILO））结构，乍看之下似乎没有什么明显的问题，它甚至可以通过你编写的各种单元测试。然而其中的pop方法却存在内存泄露的问题，当我们用pop方法弹出栈中的对象时，该对象不会被当作垃圾回收，即使使用栈的程序不再引用这些对象，因为栈内部维护着对这些对象的过期引用（obsolete reference）。在支持垃圾回收的语言中，内存泄露是很隐蔽的，这种内存泄露其实就是无意识的对象保持。如果一个对象引用被无意识的保留起来了，那么垃圾回收器不会处理这个对象，也不会处理该对象引用的其他对象，即使这样的对象只有少数几个，也可能会导致很多的对象被排除在垃圾回收之外，从而对性能造成重大影响，极端情况下会引发Disk Paging（物理内存与硬盘的虚拟内存交换数据），甚至造成OutOfMemoryError。
<br/>
### 22.抽象的（abstract）方法是否可同时是静态的（static）,是否可同时是本地方法（native），是否可同时被synchronized修饰？
答：都不能。抽象方法需要子类重写，而静态的方法是无法被重写的，因此二者是矛盾的。本地方法是由本地代码（如C代码）实现的方法，而抽象方法是没有实现的，也是矛盾的。synchronized和方法的实现细节有关，抽象方法不涉及实现细节，因此也是相互矛盾的。
<br/>
### 23.阐述 静态变量和实例变量的区别。
答：静态变量是被static修饰符修饰的变量，也称为类变量，它属于类，不属于类的任何一个对象，一个类不管创建多少个对象，静态变量在内存中有且仅有一个拷贝；实例变量必须依存于某一实例，需要先创建对象然后通过对象才能访问到它。静态变量可以实现让多个对象共享内存。在Java开发中，上下文类和工具类中通常会有大量的静态成员。
<br/>
### 24.是否可以从一个静态（static）方法内部发出对非静态（non-static）方法的调用？
答：不可以，静态方法只能访问静态成员，因为非静态方法的调用要先创建对象，在调用静态方法时可能对象并没有被初始化。
<br/>
### 25.如何实现对象克隆？
答：有两种方式：
1). 实现Cloneable接口并重写Object类中的clone()方法；
2). 实现Serializable接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深度克隆，代码如下。
``` java
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
 
public class MyUtil {
 
    private MyUtil() {
        throw new AssertionError();
    }
 
    public static <T> T clone(T obj) throws Exception {
        ByteArrayOutputStream bout = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(bout);
        oos.writeObject(obj);
 
        ByteArrayInputStream bin = new ByteArrayInputStream(bout.toByteArray());
        ObjectInputStream ois = new ObjectInputStream(bin);
        return (T) ois.readObject();
 
        // 说明：调用ByteArrayInputStream或ByteArrayOutputStream对象的close方法没有任何意义
        // 这两个基于内存的流只要垃圾回收器清理对象就能够释放资源，这一点不同于对外部资源（如文件流）的释放
    }
}
```
Example:
``` java
import java.io.Serializable;
 
/**
 * 人类
 * @author 骆昊
 *
 */
class Person implements Serializable {
    private static final long serialVersionUID = -9102017020286042305L;
 
    private String name;    // 姓名
    private int age;        // 年龄
    private Car car;        // 座驾
 
    public Person(String name, int age, Car car) {
        this.name = name;
        this.age = age;
        this.car = car;
    }
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public int getAge() {
        return age;
    }
 
    public void setAge(int age) {
        this.age = age;
    }
 
    public Car getCar() {
        return car;
    }
 
    public void setCar(Car car) {
        this.car = car;
    }
 
    @Override
    public String toString() {
        return "Person [name=" + name + ", age=" + age + ", car=" + car + "]";
    }
 
}
```
``` java
/**
 * 小汽车类
 * @author 骆昊
 *
 */
class Car implements Serializable {
    private static final long serialVersionUID = -5713945027627603702L;
 
    private String brand;       // 品牌
    private int maxSpeed;       // 最高时速
 
    public Car(String brand, int maxSpeed) {
        this.brand = brand;
        this.maxSpeed = maxSpeed;
    }
 
    public String getBrand() {
        return brand;
    }
 
    public void setBrand(String brand) {
        this.brand = brand;
    }
 
    public int getMaxSpeed() {
        return maxSpeed;
    }
 
    public void setMaxSpeed(int maxSpeed) {
        this.maxSpeed = maxSpeed;
    }
 
    @Override
    public String toString() {
        return "Car [brand=" + brand + ", maxSpeed=" + maxSpeed + "]";
    }
 
}
```
``` java
class CloneTest {
 
    public static void main(String[] args) {
        try {
            Person p1 = new Person("Hao LUO", 33, new Car("Benz", 300));
            Person p2 = MyUtil.clone(p1);   // 深度克隆
            p2.getCar().setBrand("BYD");
            // 修改克隆的Person对象p2关联的汽车对象的品牌属性
            // 原来的Person对象p1关联的汽车不会受到任何影响
            // 因为在克隆Person对象时其关联的汽车对象也被克隆了
            System.out.println(p1);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
<div class="note info"><p> 注意：基于序列化和反序列化实现的克隆不仅仅是深度克隆，更重要的是通过泛型限定，可以检查出要克隆的对象是否支持序列化，这项检查是编译器完成的，不是在运行时抛出异常，这种是方案明显优于使用Object类的clone方法克隆对象。让问题在编译的时候暴露出来总是优于把问题留到运行时。</p></div>

### 26.GC是什么？ 为什么要有GC？
答：GC是垃圾收集的意思，内存处理是编程人员容易出现问题的地方，忘记或者错误的内存回收会导致程序或系统的不稳定甚至崩溃，Java提供的GC功能可以自动监测对象是否超过作用域从而达到自动回收内存的目的，Java语言没有提供释放已分配内存的显示操作方法。Java程序员不用担心内存管理，因为垃圾收集器会自动进行管理。要请求垃圾收集，可以调用下面的方法之一：System.gc() 或Runtime.getRuntime().gc() ，但JVM可以屏蔽掉显示的垃圾回收调用。
垃圾回收可以有效的防止内存泄露，有效的使用可以使用的内存。垃圾回收器通常是作为一个单独的低优先级的线程运行，不可预知的情况下对内存堆中已经死亡的或者长时间没有使用的对象进行清除和回收，程序员不能实时的调用垃圾回收器对某个对象或所有对象进行垃圾回收。在Java诞生初期，垃圾回收是Java最大的亮点之一，因为服务器端的编程需要有效的防止内存泄露问题，然而时过境迁，如今Java的垃圾回收机制已经成为被诟病的东西。移动智能终端用户通常觉得iOS的系统比Android系统有更好的用户体验，其中一个深层次的原因就在于Android系统中垃圾回收的不可预知性。

<font size=4 color= red>补充：垃圾回收机制有很多种，包括：分代复制垃圾回收、标记垃圾回收、增量垃圾回收等方式。标准的Java进程既有栈又有堆。栈保存了原始型局部变量，堆保存了要创建的对象。Java平台对堆内存回收和再利用的基本算法被称为标记和清除，但是Java对其进行了改进，采用“分代式垃圾收集”。这种方法会跟Java对象的生命周期将堆内存划分为不同的区域，在垃圾收集过程中，可能会将对象移动到不同区域：
- 伊甸园（Eden）：这是对象最初诞生的区域，并且对大多数对象来说，这里是它们唯一存在过的区域。
- 幸存者乐园（Survivor）：从伊甸园幸存下来的对象会被挪到这里。
- 终身颐养园（Tenured）：这是足够老的幸存对象的归宿。年轻代收集（Minor-GC）过程是不会触及这个地方的。当年轻代收集不能把对象放进终身颐养园时，就会触发一次完全收集（Major-GC），这里可能还会牵扯到压缩，以便为大对象腾出足够的空间。
与垃圾回收相关的JVM参数：
-Xms / -Xmx — 堆的初始大小 / 堆的最大大小
-Xmn — 堆中年轻代的大小
-XX:-DisableExplicitGC — 让System.gc()不产生任何作用
-XX:+PrintGCDetails — 打印GC的细节
-XX:+PrintGCDateStamps — 打印GC操作的时间戳
-XX:NewSize / XX:MaxNewSize — 设置新生代大小/新生代最大大小
-XX:NewRatio — 可以设置老生代和新生代的比例
-XX:PrintTenuringDistribution — 设置每次新生代GC后输出幸存者乐园中对象年龄的分布
-XX:InitialTenuringThreshold / -XX:MaxTenuringThreshold：设置老年代阀值的初始值和最大值
-XX:TargetSurvivorRatio：设置幸存区的目标使用率 </font>
<br/>
### 27.String s = new String(“xyz”);创建了几个字符串对象？
答：两个对象，一个是静态区的”xyz”，一个是用new创建在堆上的对象。
<br/>
### 28.接口是否可继承（extends）接口？抽象类是否可实现（implements）接口？抽象类是否可继承具体类（concrete class）？
答：接口可以继承接口，而且支持多重继承。抽象类可以实现(implements)接口，抽象类可继承具体类也可以继承抽象类
<br/>
### 29.一个”.java”源文件中是否可以包含多个类（不是内部类）？有什么限制？
答：可以，但一个源文件中最多只能有一个公开类（public class）而且文件名必须和公开类的类名完全保持一致。

<br/>
### 30.内部类可以引用它的包含类（外部类）的成员吗？有没有什么限制？
答：一个内部类对象可以访问创建它的外部类对象的成员，包括私有成员。
<br/>
### 31.Anonymous Inner Class(匿名内部类)是否可以继承其它类？是否可以实现接口？
答：可以继承其他类或实现其他接口，在Swing编程和Android开发中常用此方式来实现事件监听和回调。
<br/>
### 32.Java 中的final关键字有哪些用法？
答：(1)修饰类：表示该类不能被继承；(2)修饰方法：表示方法不能被重写；(3)修饰变量：表示变量只能一次赋值以后值不能被修改（常量）。
<br/>
### 32.指出下面程序的运行结果(Static调用顺序)
``` java
class A {
 
    static {
        System.out.print("1");
    }
 
    public A() {
        System.out.print("2");
    }
}
 
class B extends A{
 
    static {
        System.out.print("a");
    }
 
    public B() {
        System.out.print("b");
    }
}
 
public class Hello {
 
    public static void main(String[] args) {
        A ab = new B();
        ab = new B();
    }
 
}
```
答：执行结果：1a2b2b。创建对象时构造器的调用顺序是：先初始化静态成员，然后调用父类构造器，再初始化非静态成员，最后调用自身构造器。
<br/>
### 33.<font color=red>如何实现字符串的反转及替换？</font>
答：方法很多，可以自己写实现也可以使用String或StringBuffer/StringBuilder中的方法。有一道很常见的面试题是用递归实现字符串反转，代码如下所示：
``` java
public static String reverse(String originStr) {
      if(originStr == null || originStr.length() <= 1) 
          return originStr;
      return reverse(originStr.substring(1)) + originStr.charAt(0);
  }
```
<br/>
### 34.Error和Exception有什么区别？
答：Error表示系统级的错误和程序不必处理的异常，是恢复不是不可能但很困难的情况下的一种严重问题；比如内存溢出，不可能指望程序能处理这样的情况；Exception表示需要捕捉或者需要程序进行处理的异常，是一种设计或实现问题；也就是说，它表示如果程序运行正常，从不会发生的情况。
Example:
``` java
class StackOverflowErrorTest {
 
    public static void main(String[] args) {
        main(null);
    }
}
```
<div class="note info"><p> 提示：用递归编写程序时一定要牢记两点：1. 递归公式；2. 收敛条件（什么时候就不再继续递归）。</p></div>

### 35.try{}里有一个return语句，那么紧跟在这个try后的finally{}里的代码会不会被执行，什么时候被执行，在return前还是后?
答：会执行，在方法返回调用者前执行。
<div class="note info"><p> 注意：在finally中改变返回值的做法是不好的，因为如果存在finally代码块，try中的return语句不会立马返回调用者，而是记录下返回值待finally代码块执行完毕之后再向调用者返回其值，然后如果在finally中修改了返回值，就会返回修改后的值。显然，在finally中返回或者修改返回值会对程序造成很大的困扰，C#中直接用编译错误的方式来阻止程序员干这种龌龊的事情，Java中也可以通过提升编译器的语法检查级别来产生警告或错误，Eclipse中可以在如图所示的地方进行设置，
强烈建议将此项设置为编译错误。</p></div>

### 36.Java语言如何进行异常处理，关键字：throws、throw、try、catch、finally分别如何使用
答：Java通过面向对象的方法进行异常处理，把各种不同的异常进行分类，并提供了良好的接口。在Java中，每个异常都是一个对象，它是Throwable类或其子类的实例。当一个方法出现异常后便抛出一个异常对象，该对象中包含有异常信息，调用这个对象的方法可以捕获到这个异常并可以对其进行处理。Java的异常处理是通过5个关键词来实现的：try、catch、throw、throws和finally。一般情况下是用try来执行一段程序，如果系统会抛出（throw）一个异常对象，可以通过它的类型来捕获（catch）它，或通过总是执行代码块（finally）来处理；try用来指定一块预防所有异常的程序；catch子句紧跟在try块后面，用来指定你想要捕获的异常的类型；throw语句用来明确地抛出一个异常；throws用来声明一个方法可能抛出的各种异常（当然声明异常时允许无病呻吟）；finally为确保一段代码不管发生什么异常状况都要被执行；try语句可以嵌套，每当遇到一个try语句，异常的结构就会被放入异常栈中，直到所有的try语句都完成。如果下一级的try语句没有对某种异常进行处理，异常栈就会执行出栈操作，直到遇到有处理这种异常的try语句或者最终将异常抛给JVM。
<br/>
### 37.运行时异常与受检异常有何异同？
答：异常表示程序运行过程中可能出现的非正常状态，运行时异常表示虚拟机的通常操作中可能遇到的异常，是一种常见运行错误，只要程序设计得没有问题通常就不会发生。受检异常跟程序运行的上下文环境有关，即使程序设计无误，仍然可能因使用的问题而引发。Java编译器要求方法必须声明抛出可能发生的受检异常，但是并不要求必须声明抛出未被捕获的运行时异常。异常和继承一样，是面向对象程序设计中经常被滥用的东西，在Effective Java中对异常的使用给出了以下指导原则：
- 不要将异常处理用于正常的控制流（设计良好的API不应该强迫它的调用者为了正常的控制流而使用异常）
- 对可以恢复的情况使用受检异常，对编程错误使用运行时异常
- 避免不必要的使用受检异常（可以通过一些状态检测手段来避免异常的发生）
- 优先使用标准的异常
- 每个方法抛出的异常都要有文档
- 保持异常的原子性
- 不要在catch中忽略掉捕获到的异常
<br/>
### 38.列出一些你常见的运行时异常？
答：- ArithmeticException（算术异常）
    - ClassCastException （类转换异常）
    - IllegalArgumentException （非法参数异常）
    - IndexOutOfBoundsException （下标越界异常）
    - NullPointerException （空指针异常）
    - SecurityException （安全异常）
<br/>
### 39.阐述final、finally、finalize的区别。
答：
- final：修饰符（关键字）有三种用法：如果一个类被声明为final，意味着它不能再派生出新的子类，即不能被继承，因此它和abstract是反义词。将变量声明为final，可以保证它们在使用中不被改变，被声明为final的变量必须在声明时给定初值，而在以后的引用中只能读取不可修改。被声明为final的方法也同样只能使用，不能在子类中被重写。
- finally：通常放在try…catch…的后面构造总是执行代码块，这就意味着程序无论正常执行还是发生异常，这里的代码只要JVM不关闭都能执行，可以将释放外部资源的代码写在finally块中。
- finalize：Object类中定义的方法，Java中允许使用finalize()方法在垃圾收集器将对象从内存中清除出去之前做必要的清理工作。这个方法是由垃圾收集器在销毁对象时调用的，通过重写finalize()方法可以整理系统资源或者执行其他清理工作。
<br/>
### 40.类ExampleA继承Exception，类ExampleB继承ExampleA。有如下代码片断：
``` java
try {
    throw new ExampleB("b")
} catch（ExampleA e）{
    System.out.println("ExampleA");
} catch（Exception e）{
    System.out.println("Exception");
}
```
请问执行此段代码的输出是什么？
答：输出：ExampleA。（根据里氏代换原则[能使用父类型的地方一定能使用子类型]，抓取ExampleA类型异常的catch块能够抓住try块中抛出的ExampleB类型的异常）
<br/>
### 41.List、Set、Map是否继承自Collection接口？
答：List、Set 是，Map 不是。Map是键值对映射容器，与List和Set有明显的区别，而Set存储的零散的元素且不允许有重复元素（数学中的集合也是如此），List是线性结构的容器，适用于按数值索引访问元素的情形。

<br/>
### 42.阐述ArrayList、Vector、LinkedList的存储性能和特性。
答：ArrayList 和Vector都是使用数组方式存储数据，此数组元素数大于实际存储的数据以便增加和插入元素，它们都允许直接按序号索引元素，但是插入元素要涉及数组元素移动等内存操作，所以索引数据快而插入数据慢，Vector中的方法由于添加了synchronized修饰，因此Vector是线程安全的容器，但性能上较ArrayList差，因此已经是Java中的遗留容器。LinkedList使用双向链表实现存储（将内存中零散的内存单元通过附加的引用关联起来，形成一个可以按序号索引的线性结构，这种链式存储方式与数组的连续存储方式相比，内存的利用率更高），按序号索引数据需要进行前向或后向遍历，但是插入数据时只需要记录本项的前后项即可，所以插入速度较快。Vector属于遗留容器（Java早期的版本中提供的容器，除此之外，Hashtable、Dictionary、BitSet、Stack、Properties都是遗留容器），已经不推荐使用，但是由于ArrayList和LinkedListed都是非线程安全的，如果遇到多个线程操作同一个容器的场景，则可以通过工具类Collections中的synchronizedList方法将其转换成线程安全的容器后再使用（这是对装潢模式的应用，将已有对象传入另一个类的构造器中创建新的对象来增强实现）。

<div class="note warning"><p> 补充：遗留容器中的Properties类和Stack类在设计上有严重的问题，Properties是一个键和值都是字符串的特殊的键值对映射，在设计上应该是关联一个Hashtable并将其两个泛型参数设置为String类型，但是Java API中的Properties直接继承了Hashtable，这很明显是对继承的滥用。这里复用代码的方式应该是Has-A关系而不是Is-A关系，另一方面容器都属于工具类，继承工具类本身就是一个错误的做法，使用工具类最好的方式是Has-A关系（关联）或Use-A关系（依赖）。同理，Stack类继承Vector也是不正确的。Sun公司的工程师们也会犯这种低级错误，让人唏嘘不已。</p></div>

### 43.Collection和Collections的区别？
答：Collection是一个接口，它是Set、List等容器的父接口；Collections是个一个工具类，提供了一系列的静态方法来辅助容器操作，这些方法包括对容器的搜索、排序、线程安全化等等。
<br/>
### 44.List、Map、Set三个接口存取元素时，各有什么特点？
答：List以特定索引来存取元素，可以有重复元素。Set不能存放重复元素（用对象的equals()方法来区分元素是否重复）。Map保存键值对（key-value pair）映射，映射关系可以是一对一或多对一。Set和Map容器都有基于哈希存储和排序树的两种实现版本，基于哈希存储的版本理论存取时间复杂度为O(1)，而基于排序树版本的实现在插入或删除元素时会按照元素或元素的键（key）构成排序树从而达到排序和去重的效果。
<br/>
### 45.TreeMap和TreeSet在排序时如何比较元素？Collections工具类中的sort()方法如何比较元素？
答：TreeSet要求存放的对象所属的类必须实现Comparable接口，该接口提供了比较元素的compareTo()方法，当插入元素时会回调该方法比较元素的大小。TreeMap要求存放的键值对映射的键必须实现Comparable接口从而根据键对元素进行排序。Collections工具类的sort方法有两种重载的形式，第一种要求传入的待排序容器中存放的对象比较实现Comparable接口以实现元素的比较；第二种不强制性的要求容器中的元素必须可比较，但是要求传入第二个参数，参数是Comparator接口的子类型（需要重写compare方法实现元素的比较），相当于一个临时定义的排序规则，其实就是通过接口注入比较元素大小的算法，也是对回调模式的应用（Java中对函数式编程的支持）。
Example1:
``` java
public class Student implements Comparable<Student> {
    private String name;        // 姓名
    private int age;            // 年龄
 
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
 
    @Override
    public String toString() {
        return "Student [name=" + name + ", age=" + age + "]";
    }
 
    @Override
    public int compareTo(Student o) {
        return this.age - o.age; // 比较年龄(年龄的升序)
    }
 
}
-----------------------------------------------------
import java.util.Set;
import java.util.TreeSet;
 
class Test01 {
 
    public static void main(String[] args) {
        Set<Student> set = new TreeSet<>();     // Java 7的钻石语法(构造器后面的尖括号中不需要写类型)
        set.add(new Student("Hao LUO", 33));
        set.add(new Student("XJ WANG", 32));
        set.add(new Student("Bruce LEE", 60));
        set.add(new Student("Bob YANG", 22));
 
        for(Student stu : set) {
            System.out.println(stu);
        }
//      输出结果: 
//      Student [name=Bob YANG, age=22]
//      Student [name=XJ WANG, age=32]
//      Student [name=Hao LUO, age=33]
//      Student [name=Bruce LEE, age=60]
    }
}
``` 
Example2:
``` java
public class Student {
    private String name;    // 姓名
    private int age;        // 年龄
 
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
 
    /**
     * 获取学生姓名
     */
    public String getName() {
        return name;
    }
 
    /**
     * 获取学生年龄
     */
    public int getAge() {
        return age;
    }
 
    @Override
    public String toString() {
        return "Student [name=" + name + ", age=" + age + "]";
    }
 
}
-----------------------------------------------------
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;
 
class Test02 {
 
    public static void main(String[] args) {
        List<Student> list = new ArrayList<>();     // Java 7的钻石语法(构造器后面的尖括号中不需要写类型)
        list.add(new Student("Hao LUO", 33));
        list.add(new Student("XJ WANG", 32));
        list.add(new Student("Bruce LEE", 60));
        list.add(new Student("Bob YANG", 22));
 
        // 通过sort方法的第二个参数传入一个Comparator接口对象
        // 相当于是传入一个比较对象大小的算法到sort方法中
        // 由于Java中没有函数指针、仿函数、委托这样的概念
        // 因此要将一个算法传入一个方法中唯一的选择就是通过接口回调
        Collections.sort(list, new Comparator<Student> () {
 
            @Override
            public int compare(Student o1, Student o2) {
                return o1.getName().compareTo(o2.getName());    // 比较学生姓名
            }
        });
 
        for(Student stu : list) {
            System.out.println(stu);
        }
//      输出结果: 
//      Student [name=Bob YANG, age=22]
//      Student [name=Bruce LEE, age=60]
//      Student [name=Hao LUO, age=33]
//      Student [name=XJ WANG, age=32]
    }
}
```
<br/>
### 46.Thread类的sleep()方法和对象的wait()方法都可以让线程暂停执行，它们有什么区别?
答：sleep()方法（休眠）是线程类（Thread）的静态方法，调用此方法会让当前线程暂停执行指定的时间，将执行机会（CPU）让给其他线程，但是对象的锁依然保持，因此休眠时间结束后会自动恢复（线程回到就绪状态，请参考第66题中的线程状态转换图）。wait()是Object类的方法，调用对象的wait()方法导致当前线程放弃对象的锁（线程暂停执行），进入对象的等待池（wait pool），只有调用对象的notify()方法（或notifyAll()方法）时才能唤醒等待池中的线程进入等锁池（lock pool），如果线程重新获得对象的锁就可以进入就绪状态。

<div class="note warning"><p> 补充：可能不少人对什么是进程，什么是线程还比较模糊，对于为什么需要多线程编程也不是特别理解。简单的说：进程是具有一定独立功能的程序关于某个数据集合上的一次运行活动，是操作系统进行资源分配和调度的一个独立单位；线程是进程的一个实体，是CPU调度和分派的基本单位，是比进程更小的能独立运行的基本单位。线程的划分尺度小于进程，这使得多线程程序的并发性高；进程在执行时通常拥有独立的内存单元，而线程之间可以共享内存。使用多线程的编程通常能够带来更好的性能和用户体验，但是多线程的程序对于其他程序是不友好的，因为它可能占用了更多的CPU资源。当然，也不是线程越多，程序的性能就越好，因为线程之间的调度和切换也会浪费CPU时间。时下很时髦的Node.js就采用了单线程异步I/O的工作模式。</p></div>

### 47.线程的sleep()方法和yield()方法有什么区别？
答：
① sleep()方法给其他线程运行机会时不考虑线程的优先级，因此会给低优先级的线程以运行的机会；yield()方法只会给相同优先级或更高优先级的线程以运行的机会；
② 线程执行sleep()方法后转入阻塞（blocked）状态，而执行yield()方法后转入就绪（ready）状态；
③ sleep()方法声明抛出InterruptedException，而yield()方法没有声明任何异常；
④ sleep()方法比yield()方法（跟操作系统CPU调度相关）具有更好的可移植性。
<br/>
### 48.当一个线程进入一个对象的synchronized方法A之后，其它线程是否可进入此对象的synchronized方法B？
答：不能。其它线程只能访问该对象的非同步方法，同步方法则不能进入。因为非静态方法上的synchronized修饰符要求执行方法时要获得对象的锁，如果已经进入A方法说明对象锁已经被取走，那么试图进入B方法的线程就只能在等锁池（注意不是等待池哦）中等待对象的锁。
<br/>
### 49.请说出与线程同步以及线程调度相关的方法。
答：
- wait()：使一个线程处于等待（阻塞）状态，并且释放所持有的对象的锁；
- sleep()：使一个正在运行的线程处于睡眠状态，是一个静态方法，调用此方法要处理InterruptedException异常；
- notify()：唤醒一个处于等待状态的线程，当然在调用此方法的时候，并不能确切的唤醒某一个等待状态的线程，而是由JVM确定唤醒哪个线程，而且与优先级无关；
- notityAll()：唤醒所有处于等待状态的线程，该方法并不是将对象的锁给所有线程，而是让它们竞争，只有获得锁的线程才能进入就绪状态；

<font color=red> 补充：</font>Java 5通过Lock接口提供了显式的锁机制（explicit lock），增强了灵活性以及对线程的协调。Lock接口中定义了加锁（lock()）和解锁（unlock()）的方法，同时还提供了newCondition()方法来产生用于线程之间通信的Condition对象；此外，Java 5还提供了信号量机制（semaphore），信号量可以用来限制对某个共享资源进行访问的线程的数量。在对资源进行访问之前，线程必须得到信号量的许可（调用Semaphore对象的acquire()方法）；在完成对资源的访问后，线程必须向信号量归还许可（调用Semaphore对象的release()方法）。
下面的例子演示了100个线程同时向一个银行账户中存入1元钱，在没有使用同步机制和使用同步机制情况下的执行情况。
银行账户类：
``` java
/**
 * 银行账户
 * @author 骆昊
 *
 */
public class Account {
    private double balance;     // 账户余额
 
    /**
     * 存款
     * @param money 存入金额
     */
    public void deposit(double money) {
        double newBalance = balance + money;
        try {
            Thread.sleep(10);   // 模拟此业务需要一段处理时间
        }
        catch(InterruptedException ex) {
            ex.printStackTrace();
        }
        balance = newBalance;
    }
 
    /**
     * 获得账户余额
     */
    public double getBalance() {
        return balance;
    }
}
```
存钱线程类：
``` java
/**
 * 存钱线程
 * @author 骆昊
 *
 */
public class AddMoneyThread implements Runnable {
    private Account account;    // 存入账户
    private double money;       // 存入金额
 
    public AddMoneyThread(Account account, double money) {
        this.account = account;
        this.money = money;
    }
 
    @Override
    public void run() {
        account.deposit(money);
    }
 
}
```
测试类：
``` java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
 
public class Test01 {
 
    public static void main(String[] args) {
        Account account = new Account();
        ExecutorService service = Executors.newFixedThreadPool(100);
 
        for(int i = 1; i <= 100; i++) {
            service.execute(new AddMoneyThread(account, 1));
        }
 
        service.shutdown();
 
        while(!service.isTerminated()) {}
 
        System.out.println("账户余额: " + account.getBalance());
    }
}
```
在没有同步的情况下，执行结果通常是显示账户余额在10元以下，出现这种状况的原因是，当一个线程A试图存入1元的时候，另外一个线程B也能够进入存款的方法中，线程B读取到的账户余额仍然是线程A存入1元钱之前的账户余额，因此也是在原来的余额0上面做了加1元的操作，同理线程C也会做类似的事情，所以最后100个线程执行结束时，本来期望账户余额为100元，但实际得到的通常在10元以下（很可能是1元哦）。解决这个问题的办法就是同步，当一个线程对银行账户存钱时，需要将此账户锁定，待其操作完成后才允许其他的线程进行操作，代码有如下几种调整方案：
在银行账户的存款（deposit）方法上同步（synchronized）关键字
``` java
/**
 * 银行账户
 * @author 骆昊
 *
 */
public class Account {
    private double balance;     // 账户余额
 
    /**
     * 存款
     * @param money 存入金额
     */
    public synchronized void deposit(double money) {
        double newBalance = balance + money;
        try {
            Thread.sleep(10);   // 模拟此业务需要一段处理时间
        }
        catch(InterruptedException ex) {
            ex.printStackTrace();
        }
        balance = newBalance;
    }
 
    /**
     * 获得账户余额
     */
    public double getBalance() {
        return balance;
    }
}
```
在线程调用存款方法时对银行账户进行同步:
``` java
/**
 * 存钱线程
 * @author 骆昊
 *
 */
public class AddMoneyThread implements Runnable {
    private Account account;    // 存入账户
    private double money;       // 存入金额
 
    public AddMoneyThread(Account account, double money) {
        this.account = account;
        this.money = money;
    }
 
    @Override
    public void run() {
        synchronized (account) {
            account.deposit(money); 
        }
    }
 
}
```
通过Java 5显示的锁机制，为每个银行账户创建一个锁对象，在存款操作进行加锁和解锁的操作
``` java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
 
/**
 * 银行账户
 * 
 * @author 骆昊
 *
 */
public class Account {
    private Lock accountLock = new ReentrantLock();
    private double balance; // 账户余额
 
    /**
     * 存款
     * 
     * @param money
     *            存入金额
     */
    public void deposit(double money) {
        accountLock.lock();
        try {
            double newBalance = balance + money;
            try {
                Thread.sleep(10); // 模拟此业务需要一段处理时间
            }
            catch (InterruptedException ex) {
                ex.printStackTrace();
            }
            balance = newBalance;
        }
        finally {
            accountLock.unlock();
        }
    }
 
    /**
     * 获得账户余额
     */
    public double getBalance() {
        return balance;
    }
}
```
按照上述三种方式对代码进行修改后，重写执行测试代码Test01，将看到最终的账户余额为100元。当然也可以使用Semaphore或CountdownLatch来实现同步。
<br/>
### 50.编写多线程程序有几种实现方式？
答：Java 5以前实现多线程有两种实现方法：一种是继承Thread类；另一种是实现Runnable接口。两种方式都要通过重写run()方法来定义线程的行为，推荐使用后者，因为Java中的继承是单继承，一个类有一个父类，如果继承了Thread类就无法再继承其他类了，显然使用Runnable接口更为灵活。
补充：Java 5以后创建线程还有第三种方式：实现Callable接口，该接口中的call方法可以在线程执行结束时产生一个返回值，代码如下所示：
``` java
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;
 
class MyTask implements Callable<Integer> {
    private int upperBounds;
 
    public MyTask(int upperBounds) {
        this.upperBounds = upperBounds;
    }
 
    @Override
    public Integer call() throws Exception {
        int sum = 0; 
        for(int i = 1; i <= upperBounds; i++) {
            sum += i;
        }
        return sum;
    }
 
}
 
class Test {
 
    public static void main(String[] args) throws Exception {
        List<Future<Integer>> list = new ArrayList<>();
        ExecutorService service = Executors.newFixedThreadPool(10);
        for(int i = 0; i < 10; i++) {
            list.add(service.submit(new MyTask((int) (Math.random() * 100))));
        }
 
        int sum = 0;
        for(Future<Integer> future : list) {
            // while(!future.isDone()) ;
            sum += future.get();
        }
 
        System.out.println(sum);
    }
}
```
<br/>
### 51.synchronized关键字的用法？
答：synchronized关键字可以将对象或者方法标记为同步，以实现对对象和方法的互斥访问，可以用synchronized(对象) { … }定义同步代码块，或者在声明方法时将synchronized作为方法的修饰符。
<br/>
### 52.启动一个线程是调用run()还是start()方法？
答：启动一个线程是调用start()方法，使线程所代表的虚拟处理机处于可运行状态，这意味着它可以由JVM 调度并执行，这并不意味着线程就会立即运行。run()方法是线程启动后要进行回调（callback）的方法。
<br/>
### 53.什么是线程池（thread pool）？
答：在面向对象编程中，创建和销毁对象是很费时间的，因为创建一个对象要获取内存资源或者其它更多资源。在Java中更是如此，虚拟机将试图跟踪每一个对象，以便能够在对象销毁后进行垃圾回收。所以提高服务程序效率的一个手段就是尽可能减少创建和销毁对象的次数，特别是一些很耗资源的对象创建和销毁，这就是”池化资源”技术产生的原因。线程池顾名思义就是事先创建若干个可执行的线程放入一个池（容器）中，需要的时候从池中获取线程不用自行创建，使用完毕不需要销毁线程而是放回池中，从而减少创建和销毁线程对象的开销。
Java 5+中的Executor接口定义一个执行线程的工具。它的子类型即线程池接口是ExecutorService。要配置一个线程池是比较复杂的，尤其是对于线程池的原理不是很清楚的情况下，因此在工具类Executors面提供了一些静态工厂方法，生成一些常用的线程池，如下所示：
- newSingleThreadExecutor：创建一个单线程的线程池。这个线程池只有一个线程在工作，也就是相当于单线程串行执行所有任务。如果这个唯一的线程因为异常结束，那么会有一个新的线程来替代它。此线程池保证所有任务的执行顺序按照任务的提交顺序执行。
- newFixedThreadPool：创建固定大小的线程池。每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。线程池的大小一旦达到最大值就会保持不变，如果某个线程因为执行异常而结束，那么线程池会补充一个新线程。
- newCachedThreadPool：创建一个可缓存的线程池。如果线程池的大小超过了处理任务所需要的线程，那么就会回收部分空闲（60秒不执行任务）的线程，当任务数增加时，此线程池又可以智能的添加新线程来处理任务。此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。
- newScheduledThreadPool：创建一个大小无限的线程池。此线程池支持定时以及周期性执行任务的需求。
- newSingleThreadExecutor：创建一个单线程的线程池。此线程池支持定时以及周期性执行任务的需求。
<br/>
### 54.线程的基本状态以及状态之间的关系？
答：![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018062801.png)
说明：其中Running表示运行状态，Runnable表示就绪状态（万事俱备，只欠CPU），Blocked表示阻塞状态，阻塞状态又有多种情况，可能是因为调用wait()方法进入等待池，也可能是执行同步方法或同步代码块进入等锁池，或者是调用了sleep()方法或join()方法等待休眠或其他线程结束，或是因为发生了I/O中断。
<br/>
### 55.简述synchronized 和java.util.concurrent.locks.Lock的异同？
答：Lock是Java 5以后引入的新的API，和关键字synchronized相比主要相同点：Lock 能完成synchronized所实现的所有功能；主要不同点：Lock有比synchronized更精确的线程语义和更好的性能，而且不强制性的要求一定要获得锁。synchronized会自动释放锁，而Lock一定要求程序员手工释放，并且最好在finally 块中释放（这是释放外部资源的最好的地方）。
<br/>
### 56.Java中如何实现序列化，有什么意义？
答：序列化就是一种用来处理对象流的机制，所谓对象流也就是将对象的内容进行流化。可以对流化后的对象进行读写操作，也可将流化后的对象传输于网络之间。序列化是为了解决对象流读写操作时可能引发的问题（如果不进行序列化可能会存在数据乱序的问题）。
要实现序列化，需要让一个类实现Serializable接口，该接口是一个标识性接口，标注该类对象是可被序列化的，然后使用一个输出流来构造一个对象输出流并通过writeObject(Object)方法就可以将实现对象写出（即保存其状态）；如果需要反序列化则可以用一个输入流建立对象输入流，然后通过readObject方法从流中读取对象。序列化除了能够实现对象的持久化之外，还能够用于对象的深度克隆。
<br/>
### 57.Java中有几种类型的流？
答：字节流和字符流。字节流继承于InputStream、OutputStream，字符流继承于Reader、Writer。在java.io 包中还有许多其他的流，主要是为了提高性能和使用方便。关于Java的I/O需要注意的有两点：一是两种对称性（输入和输出的对称性，字节和字符的对称性）；二是两种设计模式（适配器模式和装潢模式）。另外Java中的流不同于C#的是它只有一个维度一个方向。

面试题 - 编程实现文件拷贝。（这个题目在笔试的时候经常出现，下面的代码给出了两种实现方案）
``` java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.OutputStream;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;
 
public final class MyUtil {
 
    private MyUtil() {
        throw new AssertionError();
    }
 
    public static void fileCopy(String source, String target) throws IOException {
        try (InputStream in = new FileInputStream(source)) {
            try (OutputStream out = new FileOutputStream(target)) {
                byte[] buffer = new byte[4096];
                int bytesToRead;
                while((bytesToRead = in.read(buffer)) != -1) {
                    out.write(buffer, 0, bytesToRead);
                }
            }
        }
    }
 
    public static void fileCopyNIO(String source, String target) throws IOException {
        try (FileInputStream in = new FileInputStream(source)) {
            try (FileOutputStream out = new FileOutputStream(target)) {
                FileChannel inChannel = in.getChannel();
                FileChannel outChannel = out.getChannel();
                ByteBuffer buffer = ByteBuffer.allocate(4096);
                while(inChannel.read(buffer) != -1) {
                    buffer.flip();
                    outChannel.write(buffer);
                    buffer.clear();
                }
            }
        }
    }
}
```
注意：上面用到Java 7的TWR，使用TWR后可以不用在finally中释放外部资源 ，从而让代码更加优雅。

<br/>
### 58.编程题
1.写一个方法，输入一个文件名和一个字符串，统计这个字符串在这个文件中出现的次数：
``` java
import java.io.BufferedReader;
import java.io.FileReader;
 
public final class MyUtil {
 
    // 工具类中的方法都是静态方式访问的因此将构造器私有不允许创建对象(绝对好习惯)
    private MyUtil() {
        throw new AssertionError();
    }
 
    /**
     * 统计给定文件中给定字符串的出现次数
     * 
     * @param filename  文件名
     * @param word 字符串
     * @return 字符串在文件中出现的次数
     */
    public static int countWordInFile(String filename, String word) {
        int counter = 0;
        try (FileReader fr = new FileReader(filename)) {
            try (BufferedReader br = new BufferedReader(fr)) {
                String line = null;
                while ((line = br.readLine()) != null) {
                    int index = -1;
                    while (line.length() >= word.length() && (index = line.indexOf(word)) >= 0) {
                        counter++;
                        line = line.substring(index + word.length());
                    }
                }
            }
        } catch (Exception ex) {
            ex.printStackTrace();
        }
        return counter;
    }
 
}
```
2.如何用Java代码列出一个目录下所有的文件？
``` java
import java.io.File;
 
class Test12 {
 
    public static void main(String[] args) {
        File f = new File("/Users/Hao/Downloads");
        for(File temp : f.listFiles()) {
            if(temp.isFile()) {
                System.out.println(temp.getName());
            }
        }
    }
}
```
如果需要对文件夹继续展开，代码如下所示：
``` java
import java.io.File;
 
class Test12 {
 
    public static void main(String[] args) {
        showDirectory(new File("/Users/Hao/Downloads"));
    }
 
    public static void showDirectory(File f) {
        _walkDirectory(f, 0);
    }
 
    private static void _walkDirectory(File f, int level) {
        if(f.isDirectory()) {
            for(File temp : f.listFiles()) {
                _walkDirectory(temp, level + 1);
            }
        }
        else {
            for(int i = 0; i < level - 1; i++) {
                System.out.print("\t");
            }
            System.out.println(f.getName());
        }
    }
}
```
在Java 7中可以使用NIO.2的API来做同样的事情，代码如下所示：
``` java
class ShowFileTest {
 
    public static void main(String[] args) throws IOException {
        Path initPath = Paths.get("/Users/Hao/Downloads");
        Files.walkFileTree(initPath, new SimpleFileVisitor<Path>() {
 
            @Override
            public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) 
                    throws IOException {
                System.out.println(file.getFileName().toString());
                return FileVisitResult.CONTINUE;
            }
 
        });
    }
}
```
用Java的套接字编程实现一个多线程的回显（echo）服务器:
``` java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
 
public class EchoServer {
 
    private static final int ECHO_SERVER_PORT = 6789;
 
    public static void main(String[] args) {        
        try(ServerSocket server = new ServerSocket(ECHO_SERVER_PORT)) {
            System.out.println("服务器已经启动...");
            while(true) {
                Socket client = server.accept();
                new Thread(new ClientHandler(client)).start();
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
 
    private static class ClientHandler implements Runnable {
        private Socket client;
 
        public ClientHandler(Socket client) {
            this.client = client;
        }
 
        @Override
        public void run() {
            try(BufferedReader br = new BufferedReader(new InputStreamReader(client.getInputStream()));
                    PrintWriter pw = new PrintWriter(client.getOutputStream())) {
                String msg = br.readLine();
                System.out.println("收到" + client.getInetAddress() + "发送的: " + msg);
                pw.println(msg);
                pw.flush();
            } catch(Exception ex) {
                ex.printStackTrace();
            } finally {
                try {
                    client.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
 
}
```
<div class="note warning"><p>注意：上面的代码使用了Java7的TWR语法，由于很多外部资源类都间接的实现了AutoCloseable接口（单方法回调接口），因此可以利用TWR语法在try结束的时候通过回调的方式自动调用外部资源类的close()方法，避免书写冗长的finally代码块。此外，上面的代码用一个静态内部类实现线程的功能，使用多线程可以避免一个用户I/O操作所产生的中断影响其他用户对服务器的访问，简单的说就是一个用户的输入操作不会造成其他用户的阻塞。当然，上面的代码使用线程池可以获得更好的性能，因为频繁的创建和销毁线程所造成的开销也是不可忽视的。</p></div>

下面是一段回显客户端测试代码：
``` java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
import java.util.Scanner;
 
public class EchoClient {
 
    public static void main(String[] args) throws Exception {
        Socket client = new Socket("localhost", 6789);
        Scanner sc = new Scanner(System.in);
        System.out.print("请输入内容: ");
        String msg = sc.nextLine();
        sc.close();
        PrintWriter pw = new PrintWriter(client.getOutputStream());
        pw.println(msg);
        pw.flush();
        BufferedReader br = new BufferedReader(new InputStreamReader(client.getInputStream()));
        System.out.println(br.readLine());
        client.close();
    }
}
```
如果希望用NIO的多路复用套接字实现服务器，代码如下所示。NIO的操作虽然带来了更好的性能，但是有些操作是比较底层的，对于初学者来说还是有些难于理解。
``` java
import java.io.IOException;
import java.net.InetSocketAddress;
import java.nio.ByteBuffer;
import java.nio.CharBuffer;
import java.nio.channels.SelectionKey;
import java.nio.channels.Selector;
import java.nio.channels.ServerSocketChannel;
import java.nio.channels.SocketChannel;
import java.util.Iterator;
 
public class EchoServerNIO {
 
    private static final int ECHO_SERVER_PORT = 6789;
    private static final int ECHO_SERVER_TIMEOUT = 5000;
    private static final int BUFFER_SIZE = 1024;
 
    private static ServerSocketChannel serverChannel = null;
    private static Selector selector = null;    // 多路复用选择器
    private static ByteBuffer buffer = null;    // 缓冲区
 
    public static void main(String[] args) {
        init();
        listen();
    }
 
    private static void init() {
        try {
            serverChannel = ServerSocketChannel.open();
            buffer = ByteBuffer.allocate(BUFFER_SIZE);
            serverChannel.socket().bind(new InetSocketAddress(ECHO_SERVER_PORT));
            serverChannel.configureBlocking(false);
            selector = Selector.open();
            serverChannel.register(selector, SelectionKey.OP_ACCEPT);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
 
    private static void listen() {
        while (true) {
            try {
                if (selector.select(ECHO_SERVER_TIMEOUT) != 0) {
                    Iterator<SelectionKey> it = selector.selectedKeys().iterator();
                    while (it.hasNext()) {
                        SelectionKey key = it.next();
                        it.remove();
                        handleKey(key);
                    }
                }
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
 
    private static void handleKey(SelectionKey key) throws IOException {
        SocketChannel channel = null;
 
        try {
            if (key.isAcceptable()) {
                ServerSocketChannel serverChannel = (ServerSocketChannel) key.channel();
                channel = serverChannel.accept();
                channel.configureBlocking(false);
                channel.register(selector, SelectionKey.OP_READ);
            } else if (key.isReadable()) {
                channel = (SocketChannel) key.channel();
                buffer.clear();
                if (channel.read(buffer) > 0) {
                    buffer.flip();
                    CharBuffer charBuffer = CharsetHelper.decode(buffer);
                    String msg = charBuffer.toString();
                    System.out.println("收到" + channel.getRemoteAddress() + "的消息：" + msg);
                    channel.write(CharsetHelper.encode(CharBuffer.wrap(msg)));
                } else {
                    channel.close();
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
            if (channel != null) {
                channel.close();
            }
        }
    }
 
}
```
``` java
import java.nio.ByteBuffer;
import java.nio.CharBuffer;
import java.nio.charset.CharacterCodingException;
import java.nio.charset.Charset;
import java.nio.charset.CharsetDecoder;
import java.nio.charset.CharsetEncoder;
 
public final class CharsetHelper {
    private static final String UTF_8 = "UTF-8";
    private static CharsetEncoder encoder = Charset.forName(UTF_8).newEncoder();
    private static CharsetDecoder decoder = Charset.forName(UTF_8).newDecoder();
 
    private CharsetHelper() {
    }
 
    public static ByteBuffer encode(CharBuffer in) throws CharacterCodingException{
        return encoder.encode(in);
    }
 
    public static CharBuffer decode(ByteBuffer in) throws CharacterCodingException{
        return decoder.decode(in);
    }
}
```
<br/>
### 59.XML文档定义有几种形式？它们之间有何本质区别？解析XML文档有哪几种方式？
答：XML文档定义分为DTD和Schema两种形式，二者都是对XML语法的约束，其本质区别在于Schema本身也是一个XML文件，可以被XML解析器解析，而且可以为XML承载的数据定义类型，约束能力较之DTD更强大。对XML的解析主要有DOM（文档对象模型，Document Object Model）、SAX（Simple API for XML）和StAX（Java 6中引入的新的解析XML的方式，Streaming API for XML），其中DOM处理大型文件时其性能下降的非常厉害，这个问题是由DOM树结构占用的内存较多造成的，而且DOM解析方式必须在解析文件之前把整个文档装入内存，适合对XML的随机访问（典型的用空间换取时间的策略）；SAX是事件驱动型的XML解析方式，它顺序读取XML文件，不需要一次全部装载整个文件。当遇到像文件开头，文档结束，或者标签开头与标签结束时，它会触发一个事件，用户通过事件回调代码来处理XML文件，适合对XML的顺序访问；顾名思义，StAX把重点放在流上，实际上StAX与其他解析方式的本质区别就在于应用程序能够把XML作为一个事件流来处理。将XML作为一组事件来处理的想法并不新颖（SAX就是这样做的），但不同之处在于StAX允许应用程序代码把这些事件逐个拉出来，而不用提供在解析器方便时从解析器中接收事件的处理程序。
<br/>
在项目中哪些地方用到了XML？
XML的主要作用有两个方面：数据交换和信息配置。在做数据交换时，XML将数据用标签组装成起来，然后压缩打包加密后通过网络传送给接收者，接收解密与解压缩后再从XML文件中还原相关信息进行处理，XML曾经是异构系统间交换数据的事实标准，但此项功能几乎已经被JSON（JavaScript Object Notation）取而代之。当然，目前很多软件仍然使用XML来存储配置信息，我们在很多项目中通常也会将作为配置信息的硬代码写在XML文件中，Java的很多框架也是这么做的，而且这些框架都选择了dom4j作为处理XML的工具，因为Sun公司的官方API实在不怎么好用。
补充：现在有很多时髦的软件（如Sublime）已经开始将配置文件书写成JSON格式，我们已经强烈的感受到XML的另一项功能也将逐渐被业界抛弃。
<br/>
### 60.阐述JDBC操作数据库的步骤。
1.加载驱动
``` java
Class.forName("oracle.jdbc.driver.OracleDriver");
```
2.创建连接
``` java
Connection con = DriverManager.getConnection("jdbc:oracle:thin:@localhost:1521:orcl", "scott", "tiger");
```
3.创建语句
``` java
PreparedStatement ps = con.prepareStatement("select * from emp where sal between ? and ?");
ps.setInt(1, 1000);
ps.setInt(2, 3000);
```
4.执行语句
``` java
ResultSet rs = ps.executeQuery();
```
5.处理语句
``` java
while(rs.next()) {
    System.out.println(rs.getInt("empno") + " - " + rs.getString("ename"));
}
```
6.关闭资源
``` java
finally {
    if(con != null) {
        try {
            con.close();
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```
<div class="note primary"><p>提示：关闭外部资源的顺序应该和打开的顺序相反，也就是说先关闭ResultSet、再关闭Statement、在关闭Connection。上面的代码只关闭了Connection（连接），虽然通常情况下在关闭连接时，连接上创建的语句和打开的游标也会关闭，但不能保证总是如此，因此应该按照刚才说的顺序分别关闭。此外，第一步加载驱动在JDBC 4.0中是可以省略的（自动从类路径中加载驱动），但是我们建议保留。</p></div>

### 61.Statement和PreparedStatement有什么区别？哪个性能更好？
答：与Statement相比，①PreparedStatement接口代表预编译的语句，它主要的优势在于可以减少SQL的编译错误并增加SQL的安全性（减少SQL注射攻击的可能性）；②PreparedStatement中的SQL语句是可以带参数的，避免了用字符串连接拼接SQL语句的麻烦和不安全；③当批量处理SQL或频繁执行相同的查询时，PreparedStatement有明显的性能上的优势，由于数据库可以将编译优化后的SQL语句缓存起来，下次执行相同结构的语句时就会很快（不用再次编译和生成执行计划）。
<div class="note primary"><p>补充：为了提供对存储过程的调用，JDBC API中还提供了CallableStatement接口。存储过程（StoredProcedure）是数据库中一组为了完成特定功能的SQL语句的集合，经编译后存储在数据库中，用户通过指定存储过程的名字并给出参数（如果该存储过程带有参数）来执行它。虽然调用存储过程会在网络开销、安全性、性能上获得很多好处，但是存在如果底层数据库发生迁移时就会有很多麻烦，因为每种数据库的存储过程在书写上存在不少的差别。</p></div>

<br/>
### 62.使用JDBC操作数据库时，如何提升读取数据的性能？如何提升更新数据的性能？
答：要提升读取数据的性能，可以指定通过结果集（ResultSet）对象的setFetchSize()方法指定每次抓取的记录数（典型的空间换时间策略）；要提升更新数据的性能可以使用PreparedStatement语句构建批处理，将若干SQL语句置于一个批处理中执行。
<br/>
### 63.在进行数据库编程时，连接池有什么作用？
答：由于创建连接和释放连接都有很大的开销（尤其是数据库服务器不在本地时，每次建立连接都需要进行TCP的三次握手，释放连接需要进行TCP四次握手，造成的开销是不可忽视的），为了提升系统访问数据库的性能，可以事先创建若干连接置于连接池中，需要时直接从连接池获取，使用结束时归还连接池而不必关闭连接，从而避免频繁创建和释放连接所造成的开销，这是典型的用空间换取时间的策略（浪费了空间存储连接，但节省了创建和释放连接的时间）。池化技术在Java开发中是很常见的，在使用线程时创建线程池的道理与此相同。基于Java的开源数据库连接池主要有：C3P0、Proxool、DBCP、BoneCP、Druid等。
<div class="note primary"><p>补充：在计算机系统中时间和空间是不可调和的矛盾，理解这一点对设计满足性能要求的算法是至关重要的。大型网站性能优化的一个关键就是使用缓存，而缓存跟上面讲的连接池道理非常类似，也是使用空间换时间的策略。可以将热点数据置于缓存中，当用户查询这些数据时可以直接从缓存中得到，这无论如何也快过去数据库中查询。当然，缓存的置换策略等也会对系统性能产生重要影响，对于这个问题的讨论已经超出了这里要阐述的范围。</p></div>

### 64.什么是DAO模式？
答：DAO（Data Access Object）顾名思义是一个为数据库或其他持久化机制提供了抽象接口的对象，在不暴露底层持久化方案实现细节的前提下提供了各种数据访问操作。在实际的开发中，应该将所有对数据源的访问操作进行抽象化后封装在一个公共API中。用程序设计语言来说，就是建立一个接口，接口中定义了此应用程序中将会用到的所有事务方法。在这个应用程序中，当需要和数据源进行交互的时候则使用这个接口，并且编写一个单独的类来实现这个接口，在逻辑上该类对应一个特定的数据存储。DAO模式实际上包含了两个模式，一是Data Accessor（数据访问器），二是Data Object（数据对象），前者要解决如何访问数据的问题，而后者要解决的是如何用对象封装数据。
<br/>
### 65.事务的ACID是指什么？
答：
原子性(Atomic)：事务中各项操作，要么全做要么全不做，任何一项操作的失败都会导致整个事务的失败；
一致性(Consistent)：事务结束后系统状态是一致的；
隔离性(Isolated)：并发执行的事务彼此无法看到对方的中间状态；
持久性(Durable)：事务完成后所做的改动都会被持久化，即使发生灾难性的失败。通过日志和同步备份可以在故障发生后重建数据
<div class="note primary"><p>补充：关于事务，在面试中被问到的概率是很高的，可以问的问题也是很多的。首先需要知道的是，只有存在并发数据访问时才需要事务。当多个事务访问同一数据时，可能会存在5类问题，包括3类数据读取问题（脏读、不可重复读和幻读）和2类数据更新问题（第1类丢失更新和第2类丢失更新）</p></div>

<br/>
### 66.JDBC中如何进行事务处理？
答：Connection提供了事务处理的方法，通过调用setAutoCommit(false)可以设置手动提交事务；当事务完成后用commit()显式提交事务；如果在事务处理过程中发生异常则通过rollback()进行事务回滚。除此之外，从JDBC 3.0中还引入了Savepoint（保存点）的概念，允许通过代码设置保存点并让事务回滚到指定的保存点。
<br/>
### 67.JDBC能否处理Blob和Clob？
答： Blob是指二进制大对象（Binary Large Object），而Clob是指大字符对象（Character Large Objec），因此其中Blob是为存储大的二进制数据而设计的，而Clob是为存储大的文本数据而设计的。JDBC的PreparedStatement和ResultSet都提供了相应的方法来支持Blob和Clob操作。
<br/>
### 68.Java中是如何支持正则表达式操作的？
答：Java中的String类提供了支持正则表达式操作的方法，包括：matches()、replaceAll()、replaceFirst()、split()。此外，Java中可以用Pattern类表示正则表达式对象，它提供了丰富的API进行各种正则表达式操作，请参考下面面试题的代码。
面试题： - 如果要从字符串中截取第一个英文左括号之前的字符串，例如：北京市(朝阳区)(西城区)(海淀区)，截取结果为：北京市，那么正则表达式怎么写？
``` java
import java.util.regex.Matcher;
import java.util.regex.Pattern;
 
class RegExpTest {
 
    public static void main(String[] args) {
        String str = "北京市(朝阳区)(西城区)(海淀区)";
        Pattern p = Pattern.compile(".*?(?=\\()");
        Matcher m = p.matcher(str);
        if(m.find()) {
            System.out.println(m.group());
        }
    }
}
```
说明：上面的正则表达式中使用了懒惰匹配和前瞻，如果不清楚这些内容，推荐读一下网上很有名的[正则表达式30分钟入门教程](https://www.jb51.net/tools/zhengze.html)。
<br/>
### 69.获得一个类的类对象有哪些方式？
答：
方法1：类型.class，例如：String.class
方法2：对象.getClass()，例如：”hello”.getClass()
方法3：Class.forName()，例如：Class.forName(“java.lang.String”)
<br/>
### 70.如何通过反射创建对象？
答：
方法1：通过类对象调用newInstance()方法，例如：String.class.newInstance()
方法2：通过类对象的getConstructor()或getDeclaredConstructor()方法获得构造器（Constructor）对象并调用其newInstance()方法创建对象，例如：String.class.getConstructor(String.class).newInstance(“Hello”);
<br/>
### 71.如何通过反射获取和设置对象私有字段的值？
答：可以通过类对象的getDeclaredField()方法字段（Field）对象，然后再通过字段对象的setAccessible(true)将其设置为可以访问，接下来就可以通过get/set方法来获取/设置字段的值了。下面的代码实现了一个反射的工具类，其中的两个静态方法分别用于获取和设置私有字段的值，字段可以是基本类型也可以是对象类型且支持多级对象操作，例如ReflectionUtil.get(dog, “owner.car.engine.id”);可以获得dog对象的主人的汽车的引擎的ID号。
``` java
import java.lang.reflect.Constructor;
import java.lang.reflect.Field;
import java.lang.reflect.Modifier;
import java.util.ArrayList;
import java.util.List;
 
/**
 * 反射工具类
 * @author 骆昊
 *
 */
public class ReflectionUtil {
 
    private ReflectionUtil() {
        throw new AssertionError();
    }
 
    /**
     * 通过反射取对象指定字段(属性)的值
     * @param target 目标对象
     * @param fieldName 字段的名字
     * @throws 如果取不到对象指定字段的值则抛出异常
     * @return 字段的值
     */
    public static Object getValue(Object target, String fieldName) {
        Class<?> clazz = target.getClass();
        String[] fs = fieldName.split("\\.");
 
        try {
            for(int i = 0; i < fs.length - 1; i++) {
                Field f = clazz.getDeclaredField(fs[i]);
                f.setAccessible(true);
                target = f.get(target);
                clazz = target.getClass();
            }
 
            Field f = clazz.getDeclaredField(fs[fs.length - 1]);
            f.setAccessible(true);
            return f.get(target);
        }
        catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
 
    /**
     * 通过反射给对象的指定字段赋值
     * @param target 目标对象
     * @param fieldName 字段的名称
     * @param value 值
     */
    public static void setValue(Object target, String fieldName, Object value) {
        Class<?> clazz = target.getClass();
        String[] fs = fieldName.split("\\.");
        try {
            for(int i = 0; i < fs.length - 1; i++) {
                Field f = clazz.getDeclaredField(fs[i]);
                f.setAccessible(true);
                Object val = f.get(target);
                if(val == null) {
                    Constructor<?> c = f.getType().getDeclaredConstructor();
                    c.setAccessible(true);
                    val = c.newInstance();
                    f.set(target, val);
                }
                target = val;
                clazz = target.getClass();
            }
 
            Field f = clazz.getDeclaredField(fs[fs.length - 1]);
            f.setAccessible(true);
            f.set(target, value);
        }
        catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
 
}
```
<br/>
### 72.如何通过反射调用对象的方法？
``` java
import java.lang.reflect.Method;
 
class MethodInvokeTest {
 
    public static void main(String[] args) throws Exception {
        String str = "hello";
        Method m = str.getClass().getMethod("toUpperCase");
        System.out.println(m.invoke(str));  // HELLO
    }
}
```
<br/>
### 73.简述一下面向对象的”六原则一法则”。
答：
1.单一职责原则：一个类只做它该做的事情。（单一职责原则想表达的就是”高内聚”，写代码最终极的原则只有六个字”高内聚、低耦合”，就如同葵花宝典或辟邪剑谱的中心思想就八个字”欲练此功必先自宫”，所谓的高内聚就是一个代码模块只完成一项功能，在面向对象中，如果只让一个类完成它该做的事，而不涉及与它无关的领域就是践行了高内聚的原则，这个类就只有单一职责。我们都知道一句话叫”因为专注，所以专业”，一个对象如果承担太多的职责，那么注定它什么都做不好。这个世界上任何好的东西都有两个特征，一个是功能单一，好的相机绝对不是电视购物里面卖的那种一个机器有一百多种功能的，它基本上只能照相；另一个是模块化，好的自行车是组装车，从减震叉、刹车到变速器，所有的部件都是可以拆卸和重新组装的，好的乒乓球拍也不是成品拍，一定是底板和胶皮可以拆分和自行组装的，一个好的软件系统，它里面的每个功能模块也应该是可以轻易的拿到其他系统中使用的，这样才能实现软件复用的目标。）
<br/>
2.开闭原则：软件实体应当对扩展开放，对修改关闭。（在理想的状态下，当我们需要为一个软件系统增加新功能时，只需要从原来的系统派生出一些新类就可以，不需要修改原来的任何一行代码。要做到开闭有两个要点：①抽象是关键，一个系统中如果没有抽象类或接口系统就没有扩展点；②封装可变性，将系统中的各种可变因素封装到一个继承结构中，如果多个可变因素混杂在一起，系统将变得复杂而换乱，如果不清楚如何封装可变性，可以参考《设计模式精解》一书中对桥梁模式的讲解的章节。）
<br/>
3.依赖倒转原则：面向接口编程。（该原则说得直白和具体一些就是声明方法的参数类型、方法的返回类型、变量的引用类型时，尽可能使用抽象类型而不用具体类型，因为抽象类型可以被它的任何一个子类型所替代，请参考下面的里氏替换原则。）
<br/>
4.里氏替换原则：任何时候都可以用子类型替换掉父类型。（关于里氏替换原则的描述，Barbara Liskov女士的描述比这个要复杂得多，但简单的说就是能用父类型的地方就一定能使用子类型。里氏替换原则可以检查继承关系是否合理，如果一个继承关系违背了里氏替换原则，那么这个继承关系一定是错误的，需要对代码进行重构。例如让猫继承狗，或者狗继承猫，又或者让正方形继承长方形都是错误的继承关系，因为你很容易找到违反里氏替换原则的场景。需要注意的是：子类一定是增加父类的能力而不是减少父类的能力，因为子类比父类的能力更多，把能力多的对象当成能力少的对象来用当然没有任何问题。）
<br/>
5.接口隔离原则：接口要小而专，绝不能大而全。（臃肿的接口是对接口的污染，既然接口表示能力，那么一个接口只应该描述一种能力，接口也应该是高度内聚的。例如，琴棋书画就应该分别设计为四个接口，而不应设计成一个接口中的四个方法，因为如果设计成一个接口中的四个方法，那么这个接口很难用，毕竟琴棋书画四样都精通的人还是少数，而如果设计成四个接口，会几项就实现几个接口，这样的话每个接口被复用的可能性是很高的。Java中的接口代表能力、代表约定、代表角色，能否正确的使用接口一定是编程水平高低的重要标识。）
<br/>
6.合成聚合复用原则：优先使用聚合或合成关系复用代码。（通过继承来复用代码是面向对象程序设计中被滥用得最多的东西，因为所有的教科书都无一例外的对继承进行了鼓吹从而误导了初学者，类与类之间简单的说有三种关系，Is-A关系、Has-A关系、Use-A关系，分别代表继承、关联和依赖。其中，关联关系根据其关联的强度又可以进一步划分为关联、聚合和合成，但说白了都是Has-A关系，合成聚合复用原则想表达的是优先考虑Has-A关系而不是Is-A关系复用代码，原因嘛可以自己从百度上找到一万个理由，需要说明的是，即使在Java的API中也有不少滥用继承的例子，例如Properties类继承了Hashtable类，Stack类继承了Vector类，这些继承明显就是错误的，更好的做法是在Properties类中放置一个Hashtable类型的成员并且将其键和值都设置为字符串来存储数据，而Stack类的设计也应该是在Stack类中放一个Vector对象来存储数据。记住：任何时候都不要继承工具类，工具是可以拥有并可以使用的，而不是拿来继承的。）
<br/>
7.迪米特法则：迪米特法则又叫最少知识原则，一个对象应当对其他对象有尽可能少的了解。（迪米特法则简单的说就是如何做到”低耦合”，门面模式和调停者模式就是对迪米特法则的践行。对于门面模式可以举一个简单的例子，你去一家公司洽谈业务，你不需要了解这个公司内部是如何运作的，你甚至可以对这个公司一无所知，去的时候只需要找到公司入口处的前台美女，告诉她们你要做什么，她们会找到合适的人跟你接洽，前台的美女就是公司这个系统的门面。再复杂的系统都可以为用户提供一个简单的门面，Java Web开发中作为前端控制器的Servlet或Filter不就是一个门面吗，浏览器对服务器的运作方式一无所知，但是通过前端控制器就能够根据你的请求得到相应的服务。调停者模式也可以举一个简单的例子来说明，例如一台计算机，CPU、内存、硬盘、显卡、声卡各种设备需要相互配合才能很好的工作，但是如果这些东西都直接连接到一起，计算机的布线将异常复杂，在这种情况下，主板作为一个调停者的身份出现，它将各个设备连接在一起而不需要每个设备之间直接交换数据，这样就减小了系统的耦合度和复杂度，如下图所示。迪米特法则用通俗的话来将就是不要和陌生人打交道，如果真的需要，找一个自己的朋友，让他替你和陌生人打交道。）
<br/>
### 74.简述一下你了解的设计模式。
答：所谓设计模式，就是一套被反复使用的代码设计经验的总结（情境中一个问题经过证实的一个解决方案）。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。设计模式使人们可以更加简单方便的复用成功的设计和体系结构。将已证实的技术表述成设计模式也会使新系统开发者更加容易理解其设计思路。
在GoF的《Design Patterns: Elements of Reusable Object-Oriented Software》中给出了三类（创建型[对类的实例化过程的抽象化]、结构型[描述如何将类或对象结合在一起形成更大的结构]、行为型[对在不同的对象之间划分责任和算法的抽象化]）共23种设计模式，包括：Abstract Factory（抽象工厂模式），Builder（建造者模式），Factory Method（工厂方法模式），Prototype（原始模型模式），Singleton（单例模式）；Facade（门面模式），Adapter（适配器模式），Bridge（桥梁模式），Composite（合成模式），Decorator（装饰模式），Flyweight（享元模式），Proxy（代理模式）；Command（命令模式），Interpreter（解释器模式），Visitor（访问者模式），Iterator（迭代子模式），Mediator（调停者模式），Memento（备忘录模式），Observer（观察者模式），State（状态模式），Strategy（策略模式），Template Method（模板方法模式）， Chain Of Responsibility（责任链模式）。
面试被问到关于设计模式的知识时，可以拣最常用的作答，例如：
- 工厂模式：工厂类可以根据条件生成不同的子类实例，这些子类有一个公共的抽象父类并且实现了相同的方法，但是这些方法针对不同的数据进行了不同的操作（多态方法）。当得到子类的实例后，开发人员可以调用基类中的方法而不必考虑到底返回的是哪一个子类的实例。
- 代理模式：给一个对象提供一个代理对象，并由代理对象控制原对象的引用。实际开发中，按照使用目的的不同，代理可以分为：远程代理、虚拟代理、保护代理、Cache代理、防火墙代理、同步化代理、智能引用代理。
- 适配器模式：把一个类的接口变换成客户端所期待的另一种接口，从而使原本因接口不匹配而无法在一起使用的类能够一起工作。
- 模板方法模式：提供一个抽象类，将部分逻辑以具体方法或构造器的形式实现，然后声明一些抽象方法来迫使子类实现剩余的逻辑。不同的子类可以以不同的方式实现这些抽象方法（多态实现），从而实现不同的业务逻辑。
除此之外，还可以讲讲上面提到的门面模式、桥梁模式、单例模式、装潢模式（Collections工具类和I/O系统中都使用装潢模式）等，反正基本原则就是拣自己最熟悉的、用得最多的作答，以免言多必失。
<br/>
### 75.用Java写一个单例类。
1.饿汉式单例
``` java
public class Singleton {
    private Singleton(){}
    private static Singleton instance = new Singleton();
    public static Singleton getInstance(){
        return instance;
    }
}
```
2.懒汉式单例
``` java
public class Singleton {
    private static Singleton instance = null;
    private Singleton() {}
    public static synchronized Singleton getInstance(){
        if (instance == null) instance ＝ new Singleton();
        return instance;
    }
}
```
<div class="note info"><p>注意：实现一个单例有两点注意事项，①将构造器私有，不允许外界通过构造器创建对象；②通过公开的静态方法向外界返回类的唯一实例。这里有一个问题可以思考：Spring的IoC容器可以为普通的类创建单例，它是怎么做到的呢？</p></div>

<br/>
### 76.什么是UML？
答：UML是统一建模语言（Unified Modeling Language）的缩写，它发表于1997年，综合了当时已经存在的面向对象的建模语言、方法和过程，是一个支持模型化和软件系统开发的图形化语言，为软件开发的所有阶段提供模型化和可视化支持。使用UML可以帮助沟通与交流，辅助应用设计和文档的生成，还能够阐释系统的结构和行为。
<br/>
### 77.UML中有哪些常用的图？
UML定义了多种图形化的符号来描述软件系统部分或全部的静态结构和动态结构，包括：用例图（use case diagram）、类图（class diagram）、时序图（sequence diagram）、协作图（collaboration diagram）、状态图（statechart diagram）、活动图（activity diagram）、构件图（component diagram）、部署图（deployment diagram）等。在这些图形化符号中，有三种图最为重要，分别是：用例图（用来捕获需求，描述系统的功能，通过该图可以迅速的了解系统的功能模块及其关系）、类图（描述类以及类与类之间的关系，通过该图可以快速了解系统）、时序图（描述执行特定任务时对象之间的交互关系以及执行顺序，通过该图可以了解对象能接收的消息也就是说对象能够向外界提供的服务）。

## <div class="note default"><p>框架篇</p></div>
### 1.spring mvc 工作机制(原理):
![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018062901.png)
Spring mvc运行原理

　　1.springmvc将所有的请求都提交给DispatcherServlet,它会委托应用系统的其他模块负责对请求进行真正的处理工作。

　　2.DispatcherServlet查询一个或多个HandlerMapping,找到处理请求的Controller.

　　3.DispatcherServlet将请求提交到目标Controller

　　4.Controller进行业务逻辑处理后，会返回一个ModelAndView

　　5.DispathcherServlet查询一个或多个ViewResolver视图解析器,找到ModelAndView对象指定的视图对象;

　　6.视图负责将结果显示到客户端;

　　视图对象负责渲染返回给客户端。
<br/>
DispatcherServlet是整个Spring MVC的核心。它负责接收HTTP请求组织协调Spring MVC的各个组成部分。其主要工作有以下三项：
　　1. 截获符合特定格式的URL请求。

　　2. 初始化DispatcherServlet上下文对应的WebApplicationContext，并将其与业务层、持久化层的WebApplicationContext建立关联。
　　3. 初始化Spring MVC的各个组成组件，并装配到DispatcherServlet中。
### 2.锁机制有什么用？简述Hibernate的悲观锁和乐观锁机制。
有些业务逻辑在执行过程中要求对数据进行排他性的访问，于是需要通过一些机制保证在此过程中数据被锁住不会被外界修改，这就是所谓的锁机制。 1. 悲观锁，顾名思义悲观的认为在数据处理过程中极有可能存在修改数据的并发事务（包括本系统的其他事务或来自外部系统的事务），于是将处理的数据设置为锁定状态。悲观锁必须依赖数据库本身的锁机制才能真正保证数据访问的排他性 2. 乐观锁，顾名思义，对并发事务持乐观态度（认为对数据的并发操作不会经常性的发生），通过更加宽松的锁机制来解决由于悲观锁排他性的数据访问对系统性能造成的严重影响。最常见的乐观锁是通过数据版本标识来实现的，读取数据时获得数据的版本号，更新数据时将此版本号加1，然后和数据库表对应记录的当前版本号进行比较，如果提交的数据版本号大于数据库中此记录的当前版本号则更新数据，否则认为是过期数据无法更新。 * Hibernate中通过Session的get()和load()方法从数据库中加载对象时可以通过参数指定使用悲观锁；而乐观锁可以通过给实体类加整型的版本字段再通过XML或@Version注解进行配置。

### 3.阐述Hibernate实体对象的三种状态以及转换关系。
瞬时态：当new一个实体对象后，这个对象处于瞬时态，即这个对象只是一个保存临时数据的内存区域，如果没有变量引用这个对象，则会被JVM的垃圾回收机制回收。这个对象所保存的数据与数据库没有任何关系，除非通过Session的save()、saveOrUpdate()、persist()、merge()方法把瞬时态对象与数据库关联，并把数据插入或者更新到数据库，这个对象才转换为持久态对象。<br/>
持久态：持久态对象的实例在数据库中有对应的记录，并拥有一个持久化标识（ID）。对持久态对象进行delete操作后，数据库中对应的记录将被删除，那么持久态对象与数据库记录不再存在对应关系，持久态对象变成移除态（可以视为瞬时态）。持久态对象被修改变更后，不会马上同步到数据库，直到数据库事务提交。<br/>
游离态：当Session进行了close()、clear()、evict()或flush()后，实体对象从持久态变成游离态，对象虽然拥有持久和与数据库对应记录一致的标识值，但是因为对象已经从会话中清除掉，对象不在持久化管理之内，所以处于游离态（也叫脱管态）。游离态的对象与临时状态对象是十分相似的，只是它还含有持久化标识。

### 4.使用Spring框架的好处是什么？
- 轻量：Spring 是轻量的，基本的版本大约2MB。
- 控制反转：Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。
- 面向切面的编程(AOP)：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。
- 容器：Spring 包含并管理应用中对象的生命周期和配置。
MVC框架：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
- 事务管理：Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。
- 异常处理：Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常。

### 5.什么是Spring beans?
Spring beans 是那些形成Spring应用的主干的java对象。它们被Spring IOC容器初始化，装配，和管理。这些beans通过容器中配置的元数据创建。比如，以XML文件中<bean/> 的形式定义。
Spring 框架定义的beans都是单件beans。在bean tag中有个属性”singleton”，如果它被赋为TRUE，bean 就是单件，否则就是一个 prototype bean。默认是TRUE，所以所有在Spring框架中的beans 缺省都是单件。

### 6.Spring框架分为哪七大模块，各模块的主要功能作用是什么？
- 1.Spring Core： Core封装包是框架的最基础部分，提供IOC和依赖注入特性。这里的基础概念是BeanFactory，它提供对Factory模式的经典实现来消除对程序性单例模式的需要，并真正地允许你从程序逻辑中分离出依赖关系和配置。
- 2.Spring Context: 构建于Core封装包基础上的 Context 封装包，提供了一种框架式的对象访问方法，有些象JNDI注册器。Context封装包的特性得自于Beans封装包，并添加了对国际化（I18N）的支持（例如资源绑定），事件传播，资源装载的方式和Context的透明创建，比如说通过Servlet容器。
- 3.Spring DAO: DAO (Data Access Object)提供了JDBC的抽象层，它可消除冗长的JDBC编码和解析数据库厂商特有的错误代码。 并且，JDBC封装包还提供了一种比编程性更好的声明性事务管理方法，不仅仅是实现了特定接口，而且对所有的POJOs（plain old Java objects）都适用。
- 4.Spring ORM:ORM 封装包提供了常用的“对象/关系”映射APIs的集成层。 其中包括 JPA 、 JDO 、 Hibernate 和 iBatis 。利用ORM封装包，可以混合使用所有Spring提供的特性进行“对象/关系”映射，如前边提到的简单声明性事务管理。
- 5.Spring AOP: Spring的 AOP 封装包提供了符合AOP Alliance规范的面向方面的编程实现，让你可以定义，例如方法拦截器（method-interceptors）和切点（pointcuts），从逻辑上讲，从而减弱代码的功能耦合，清晰的被分离开。而且，利用source-level的元数据功能，还可以将各种行为信息合并到你的代码中。
- 6.Spring Web: Spring中的 Web 包提供了基础的针对Web开发的集成特性，例如多方文件上传，利用Servlet listeners进行IOC容器初始化和针对Web的ApplicationContext。当与WebWork或Struts一起使用Spring时，这个包使Spring可与其他框架结合。
- 7.Spring Web MVC: Spring中的MVC封装包提供了Web应用的Model-View-Controller（MVC）实现。Spring的MVC框架并不是仅仅提供一种传统的实现，它提供了一种清晰的分离模型，在领域模型代码和Web Form之间。并且，还可以借助Spring框架的其他特性。

### 7.Spring框架中的三大核心思想是什么
DI(依赖注入),IOC(控制反转),AOP(面向切面编程)

### 8.IOC的概念以及在Spring容器中如何进行IOC的操作
IOC：Inversion of Control，控制反转模式（也称作依赖性介入）的基本概念是：不创建对象，但是描述创建它们的方式。在代码中不直接与对象和服务连接，但在配置文件中描述哪一个组件需要哪一项服务。容器 （在 Spring 框架中是 IOC 容器） 负责将这些联系在一起。
在Java开发中，IOC意味着将你设计好的类交给系统去控制，而不是在你的类内部控制，这称为控制反转，就是被调用类的实例由原先的调用类控制创建、销毁现在转变成由Spring的容器管理。

### 9.DI的概念以及在Spring框架注入有几种方式。使用构造注入对象，必须要注意什么问题，当设值注入与构造注入同时存在时，执行的先后流程顺序?
注入方式： 1. 接口注入 2. 属性注入[属性的SET/GET] 3. 构造注入[构造方法注入] 使用构造函数依赖注入时，Spring保证对象其所依赖的所有对象先实例化后，才实例化这个对象。 使用set方法依赖注入时，Spring首先实例化对象，然后才实例化所有依赖的对象。 * 当设值注入与构造注入同时存在时，先执行设置注入，在执行构造注入。

### 10.使用DI注入时，Property代表什么意思，如果property引用的是其他Bean的话，如何注入，如果引用是字符串的话，如何设置？
使用DI注入时，Property代表注入类的属性，如果引用其他的bean，则用ref属性来表明被引用bean的名称，如果是引用字符串的话，用value属性。
如： 
``` java
<property name=”userDao” ref=”被引用bean的名称” /> <property name=”username” value = ”字符串”/>
```
### 11.在使用Spring的JDBCTemplate操作数据时，必须要往模板中注入哪些对象，同时模板要不要手动关闭数据库连接
注入DataSource数据源对象,不需要手动关闭数据库连接，JdbcTemplate会帮我们关闭数据库连接

### 12.AOP的概念以及使用AOP机制有什么好处。Java编程中实现AOP有几种方式？
- AOP的概念是Aspected Oriented Programming 面向切面编程, AOP将程序分解成各个切面或者说关注点。这使得可以模块化，相当横向上分切了。它可以解决OOP和过程化方法不能够很好解决的横切（crosscut）问题，如：事务、安全、日志等横切关注。

- 实现AOP有几种方式: >1. Spring 1.2版本中通过ProxyFactoryBean来实现aop,即通过动态代理来实现的,Aspect必须继承MethodBeforeAdvice,MethodAfterAdvice等 >2. Spring 2.0 AOP需要改的是FBI 这个类，而且它也不需要再实现某些接口 >3. 使用标注（@AspectJ）实现AOP

### 13.Hibernate中Session的save()、update()、merge()、lock()、saveOrUpdate()和persist()方法分别是做什么的？有什么区别？
Hibernate的对象有三种状态：瞬时态（transient）、持久态（persistent）和游离态（detached）。

- 1.瞬时态的实例可以通过调用save()、persist()或者saveOrUpdate()方法变成持久态。
- 2.游离态的实例可以通过调用 update()、saveOrUpdate()、lock()或者replicate()变成持久态。
- 3.save()和persist()将会引发SQL的INSERT语句，而update()或merge()会引发UPDATE语句。save()和update()的区别在于一个是将瞬时态对象变成持久态，一个是将游离态对象变为持久态。merge()方法可以完成save()和update()方法的功能，它的意图是将新的状态合并到已有的持久化对象上或创建新的持久化对象。 
- 4.persist()方法把一个瞬时态的实例持久化，但是并不保证标识符被立刻填入到持久化实例中，标识符的填入可能被推迟到flush的时间； persist()方法保证当它在一个事务外部被调用的时候并不触发一个INSERT语句，当需要封装一个长会话流程的时候，persist()方法是很有必要的；save()方法它要返回标识符，所以它会立即执行INSERT语句，不管是在事务内部还是外部。
- 5.至于lock()方法和update()方法的区别，update()方法是把一个已经更改过的脱管状态的对象变成持久状态；lock()方法是把一个没有更改过的脱管状态的对象变成持久状态。

### 14.阐述Hibernate实体对象的三种状态以及转换关系。
- 瞬时态：当new一个实体对象后，这个对象处于瞬时态，即这个对象只是一个保存临时数据的内存区域，如果没有变量引用这个对象，则会被JVM的垃圾回收机制回收。这个对象所保存的数据与数据库没有任何关系，除非通过Session的save()、saveOrUpdate()、persist()、merge()方法把瞬时态对象与数据库关联，并把数据插入或者更新到数据库，这个对象才转换为持久态对象。
- 持久态：持久态对象的实例在数据库中有对应的记录，并拥有一个持久化标识（ID）。对持久态对象进行delete操作后，数据库中对应的记录将被删除，那么持久态对象与数据库记录不再存在对应关系，持久态对象变成移除态（可以视为瞬时态）。持久态对象被修改变更后，不会马上同步到数据库，直到数据库事务提交。
- 游离态：当Session进行了close()、clear()、evict()或flush()后，实体对象从持久态变成游离态，对象虽然拥有持久和与数据库对应记录一致的标识值，但是因为对象已经从会话中清除掉，对象不在持久化管理之内，所以处于游离态（也叫脱管态）。游离态的对象与临时状态对象是十分相似的，只是它还含有持久化标识。

### 15.锁机制有什么用？简述Hibernate的悲观锁和乐观锁机制。
有些业务逻辑在执行过程中要求对数据进行排他性的访问，于是需要通过一些机制保证在此过程中数据被锁住不会被外界修改，这就是所谓的锁机制。 1. 悲观锁，顾名思义悲观的认为在数据处理过程中极有可能存在修改数据的并发事务（包括本系统的其他事务或来自外部系统的事务），于是将处理的数据设置为锁定状态。悲观锁必须依赖数据库本身的锁机制才能真正保证数据访问的排他性 2. 乐观锁，顾名思义，对并发事务持乐观态度（认为对数据的并发操作不会经常性的发生），通过更加宽松的锁机制来解决由于悲观锁排他性的数据访问对系统性能造成的严重影响。最常见的乐观锁是通过数据版本标识来实现的，读取数据时获得数据的版本号，更新数据时将此版本号加1，然后和数据库表对应记录的当前版本号进行比较，如果提交的数据版本号大于数据库中此记录的当前版本号则更新数据，否则认为是过期数据无法更新。 * Hibernate中通过Session的get()和load()方法从数据库中加载对象时可以通过参数指定使用悲观锁；而乐观锁可以通过给实体类加整型的版本字段再通过XML或@Version注解进行配置。

### 16.Spring框架中的事务处理有几种。请分别阐述两者的区别
spring提供的事务管理可以分为两类：编程式的和声明式的。编程式的，比较灵活，但是代码量大，存在重复的代码比较多;声明式的比编程式的更灵活.

### 17.Spring的ApplicationContext通常的实现是什么?
Application Context 是 spring 中较高级的容器。和 BeanFactory 类似，它可以加载配置文件中定义的 bean，将所有的 bean 集中在一起，当有请求的时候分配 bean。 另外，它增加了企业所需要的功能，比如，从属性文件从解析文本信息和将事件传递给所指定的监听器。这个容器在 org.springframework.context.ApplicationContext interface 接口中定义。

ApplicationContext 包含 BeanFactory 所有的功能，一般情况下，相对于 BeanFactory，ApplicationContext 会被推荐使用。BeanFactory 仍然可以在轻量级应用中使用，比如移动设备或者基于 applet 的应用程序。

- FileSystemXmlApplicationContext ：此容器从一个XML文件中加载beans的定义，XML Bean 配置文件的全路径名必须提供给它的构造函数。
- ClassPathXmlApplicationContext：此容器也从一个XML文件中加载beans的定义，这里，你需要正确设置classpath因为这个容器将在classpath里找bean配置。
- WebXmlApplicationContext：此容器加载一个XML文件，此文件定义了一个WEB应用的所有bean。

### 18.Spring常用注解有哪些
- 1.@Required 注释应用于 bean 属性的 setter 方法，它表明受影响的 bean 属性在配置时必须放在 XML 配置文件中，否则容器就会抛出一个 BeanInitializationException 异常。
- 2.@Autowired 注解提供了更细粒度的控制，包括在何处以及如何完成自动装配。它的用法和@Required一样，修饰setter方法、构造器、属性或者具有任意名称和/或多个参数的PN方法。
- 3.@Qualifier 注解当有多个相同类型的bean却只有一个需要自动装配时，将@Qualifier 注解和@Autowire 注解结合使用以消除这种混淆，指定需要装配的确切的bean。

### 19.Spring支持的事务管理类型
- 1.编程式事务管理：这意味你通过编程的方式管理事务，给你带来极大的灵活性，但是难维护。
- 2.声明式事务管理：这意味着你可以将业务代码和事务管理分离，你只需用注解和XML配置来管理事务。 大多数Spring框架的用户选择声明式事务管理，因为它对应用代码的影响最小，因此更符合一个无侵入的轻量级容器的思想。声明式事务管理要优于编程式事务管理，虽然比编程式事务管理（这种方式允许你通过代码控制事务）少了一点灵活性。

### 20.使用Spring通过什么方式访问Hibernate?
在Spring中有两种方式访问Hibernate： 1. 控制反转 Hibernate Template和 Callback。 2. 继承 HibernateDAOSupport提供一个AOP 拦截器。
用Spring的 SessionFactory 调用 LocalSessionFactory。集成过程分三步： 

- 1.配置the Hibernate SessionFactory。 
- 2.继承HibernateDaoSupport实现一个DAO。 
- 3.在AOP支持的事务中装配。

### 21.可以在Spring中注入一个null 和一个空字符串吗？
可以。

### 22.什么是基于Java的Spring注解配置? 给一些注解的例子.
基于Java的配置，允许你在少量的Java注解的帮助下，进行你的大部分Spring配置而非通过XML文件。
以@Configuration 注解为例，它用来标记类可以当做一个bean的定义，被Spring IOC容器使用。另一个例子是@Bean注解，它表示此方法将要返回一个对象，作为一个bean注册进Spring应用上下文。

### 23.解释不同方式的自动装配 。
有五种自动装配的方式，可以用来指导Spring容器用自动装配方式来进行依赖注入。

- 1.no：默认的方式是不进行自动装配，通过显式设置ref 属性来进行装配。 
- 2.byName：通过参数名 自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byname，之后容器试图匹配、装配和该bean的属性具有相同名字的bean。 
- 3.byType:：通过参数类型自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byType，之后容器试图匹配、装配和该bean的属性具有相同类型的bean。如果有多个bean符合条件，则抛出错误。 
- 4.constructor：这个方式类似于byType， 但是要提供给构造器参数，如果没有确定的带参数的构造器参数类型，将会抛出异常。 
- 5.autodetect：首先尝试使用constructor来自动装配，如果无法工作，则使用byType方式。

### 24.在 Spring中如何注入一个java集合？
Spring提供以下几种集合的配置元素： 

- 1.<list>类型用于注入一列值，允许有相同的值。 
- 2.<<set> 类型用于注入一组值，不允许有相同的值。 
- 3.<<map> 类型用于注入一组键值对，键和值都可以为任意类型。 
- 4.<<props>类型用于注入一组键值对，键和值都只能为String类型。

### 25.Hibernate中Session的load和get方法的区别是什么？
- 1.如果没有找到符合条件的记录，get方法返回null，load方法抛出异常。
- 2.get方法直接返回实体类对象，load方法返回实体类对象的代理。
- 3.在Hibernate 3之前，get方法只在一级缓存中进行数据查找，如果没有找到对应的数据则越过二级缓存，直接发出SQL语句完成数据读取；load方法则可以从二级缓存中获取数据；从Hibernate 3开始，get方法不再是对二级缓存只写不读，它也是可以访问二级缓存的。

### 26.struts1与struts2的区别？
struts1是基于JSP和servlet的一个开源的Web应用框架，使用的是MVC的设计模式。struts2是基于webwork技术的框架，是sun和webwork公司联手开发的一个功能非常齐全的框架，struts2和struts1没有任何关系，是一个全新的框架。

### 27.JSF是什么？
JavaServer Face是基于组件的web开发框架，跟sturts差不多的框架

### 28.jsp有哪些内置对象?作用分别是什么?
- 1.request
- 2.response
- 3.session
- 4.pageContext,javax.servlet.jsp.PageContext 的实例，对象直译时可以称作“页面上下文”对象，代表的是当前页面运行的一些属性，通过此对象可以拿到其他8大对象，使用该对象可以访问页面中的共享数据。常用的方法有getServletContextO和getServletConfigO等。
- 5.page,对应this关键字，JSP网页本身。page对象是当前页面转换后的Servlet类的实例。从转换后的Servlet类的代码中，可以看到这种关系： <br/> Object page = this; <br/> 在JSP页面中，很少使用page对象。
- 6.config >>javax.servlet. ServletConfig 的实例，该实例代表该JSP 的配置信息。常用的方法有getInitPararneter(String paramNarne) 及getInitPararneternarnes() 等方法。事实上， JSP 页面通常无须配置，也就不存在配置信息。因此，该对象更多地在Servlet 中有效。
- 7.application >>服务器启动后就产生了这个application对象，当客户在所访问的网站的各个页面之间浏览时，这个application对象都是同一个，直到服务器关闭。但是与session不同的是，所有客户的application对象都是同一个，即所有客户共享这个内置的application对象。
- 8.exception >>该实例代表其他页面中的异常和错误。只有当页面是错误处理页面，即编译指令page 的isErrorPage 属性为true 时，该对象才可以使用。常用的方法有getMessageO和printStackTraceO等。
- 9.out >>out对象是一个输出流，用来向客户端输出数据。out对象用于各种数据的输出。

### 29.jsp静态包含和动态包含的区别?
- <%@ include file=”” %>是指令元素，静态包含。<jsp:include page=”” />是行为元素，动态包含。
- 最终编译成java文件的数目不同。<br/>1. 静态包含在转换成为java文件的时候将包含文件的内容“复制”到主体文件，然后作为一个整体编译。生成一个以包含页面命名的servlet和class文件。<br/>2. 动态包含是各个jsp文件分别转换，分别编译。最终编程成多个java文件。
- 执行时间不同：静态包含发生在：JSP生成class文件阶段。 动态包含发生在：执行class文件阶段。动态加入。
- 静态包含的两个文件中不能有相同的变量，动态包含允许。
- 无论是动态包含还是静态包含，其request对象都是相同的。也就是同一个request对象。

### 30.Spring的声明式事务能不能为普通的类产生代理接口，能不能在代码中使用Try/Catch能捕获异常，如果不可以，请说明原因
不能。Spring的声明式事务为实现类产生代理。不能在代码中使用Try/Catch，因为代码中捕获了异常，Spring容器捕获不了异常。

## <div class="note default"><p>算法篇</p></div>
### 1.用Java写一个冒泡排序。
冒泡排序几乎是个程序员都写得出来，但是面试的时候如何写一个逼格高的冒泡排序却不是每个人都能做到，下面提供一个参考代码：
``` java
import java.util.Comparator;
 
/**
 * 排序器接口(策略模式: 将算法封装到具有共同接口的独立的类中使得它们可以相互替换)
 * @author骆昊
 *
 */
public interface Sorter {
 
   /**
    * 排序
    * @param list 待排序的数组
    */
   public <T extends Comparable<T>> void sort(T[] list);
 
   /**
    * 排序
    * @param list 待排序的数组
    * @param comp 比较两个对象的比较器
    */
   public <T> void sort(T[] list, Comparator<T> comp);
}
```
``` java
import java.util.Comparator;
 
/**
 * 冒泡排序
 * 
 * @author骆昊
 *
 */
public class BubbleSorter implements Sorter {
 
    @Override
    public <T extends Comparable<T>> void sort(T[] list) {
        boolean swapped = true;
        for (int i = 1, len = list.length; i < len && swapped; ++i) {
            swapped = false;
            for (int j = 0; j < len - i; ++j) {
                if (list[j].compareTo(list[j + 1]) > 0) {
                    T temp = list[j];
                    list[j] = list[j + 1];
                    list[j + 1] = temp;
                    swapped = true;
                }
            }
        }
    }
 
    @Override
    public <T> void sort(T[] list, Comparator<T> comp) {
        boolean swapped = true;
        for (int i = 1, len = list.length; i < len && swapped; ++i) {
            swapped = false;
            for (int j = 0; j < len - i; ++j) {
                if (comp.compare(list[j], list[j + 1]) > 0) {
                    T temp = list[j];
                    list[j] = list[j + 1];
                    list[j + 1] = temp;
                    swapped = true;
                }
            }
        }
    }
}
```
### 2.用Java写一个折半查找。
折半查找，也称二分查找、二分搜索，是一种在有序数组中查找某一特定元素的搜索算法。搜素过程从数组的中间元素开始，如果中间元素正好是要查找的元素，则搜素过程结束；如果某一特定元素大于或者小于中间元素，则在数组大于或小于中间元素的那一半中查找，而且跟开始一样从中间元素开始比较。如果在某一步骤数组已经为空，则表示找不到指定的元素。这种搜索算法每一次比较都使搜索范围缩小一半，其时间复杂度是O(logN)。
``` java
import java.util.Comparator;
 
public class MyUtil {
 
   public static <T extends Comparable<T>> int binarySearch(T[] x, T key) {
      return binarySearch(x, 0, x.length- 1, key);
   }
 
   // 使用循环实现的二分查找
   public static <T> int binarySearch(T[] x, T key, Comparator<T> comp) {
      int low = 0;
      int high = x.length - 1;
      while (low <= high) {
          int mid = (low + high) >>> 1;
          int cmp = comp.compare(x[mid], key);
          if (cmp < 0) {
            low= mid + 1;
          }
          else if (cmp > 0) {
            high= mid - 1;
          }
          else {
            return mid;
          }
      }
      return -1;
   }
 
   // 使用递归实现的二分查找
   private static<T extends Comparable<T>> int binarySearch(T[] x, int low, int high, T key) {
      if(low <= high) {
        int mid = low + ((high -low) >> 1);
        if(key.compareTo(x[mid])== 0) {
           return mid;
        }
        else if(key.compareTo(x[mid])< 0) {
           return binarySearch(x,low, mid - 1, key);
        }
        else {
           return binarySearch(x,mid + 1, high, key);
        }
      }
      return -1;
   }
}
```
### 3.斐波纳契数列
``` java
public class Solution {
    /*
     * @param : an integer
     * @return: an ineger f(n)
     */
    public int fibonacci(int n) {
        // write your code here
        // if (n == 1 || n == 2)  
        //     return n ==1?0:1;  
        // else  
        //     return fibonacci(n - 1) + fibonacci(n- 2);
        
        int a =0,b =1,c =0;
        int i= 2;
         if (n == 1 || n == 2){
            return n ==1?0:1; 
        }else{
            for(;i<n;i++){
                c=a+b;
                a=b;
                b=c;
            }  
        }  
        return c;
    }
};
```

### 4.K个最近的点:<br/>给定一些 points 和一个 origin，从 points 中找到 k 个离 origin 最近的点。按照距离由小到大返回。如果两个点有相同距离，则按照x值来排序；若x值也相同，就再按照y值排序。

``` java
/**
 * Definition for a point.
 * class Point {
 *     int x;
 *     int y;
 *     Point() { x = 0; y = 0; }
 *     Point(int a, int b) { x = a; y = b; }
 * }
 */
public class Solution {
    /**
     * @param points a list of points
     * @param origin a point
     * @param k an integer
     * @return the k closest points
     */
    public Point[] kClosest(Point[] points, Point origin, int k) {
        // Write your code here
         Point[] outputPoints = new Point[k];  
         for(int i=0;i<points.length;i++){  
            for (int j=i;j<points.length;j++){  
                if(length(points[i],origin)>length(points[j],origin)){  
                    Point temp=points[i];  
                    points[i]=points[j];  
                    points[j]=temp;  
                }else if(length(points[i],origin)==length(points[j],origin)){  
                    if(points[i].x>points[j].x){  
                        Point temp=points[i];  
                        points[i]=points[j];  
                        points[j]=temp;  
                    }else if(points[i].y>points[j].y){  
                        Point temp=points[i];  
                        points[i]=points[j];  
                        points[j]=temp;  
                    }  
                }  
            }  
        }  
        for (int i=0;i<k;i++){  
            outputPoints[i]=points[i];  
        }  
        return outputPoints;  
    }
    
    public static double length(Point start,Point end){  
        int temp_x=Math.abs(start.x-end.x);  
        int temp_y=Math.abs(start.y-end.y);  
        return Math.sqrt(temp_x*temp_x+temp_y*temp_y);          
    }  
}
```
### 5.二叉树的最大节点:<br/>在二叉树中寻找值最大的节点并返回
``` java
public class Solution {
    /**
     * @param root the root of binary tree
     * @return the max ndoe
     */
    public TreeNode maxNode(TreeNode root) {
        // Write your code here
        TreeNode max = new TreeNode(-1000);
        if (root == null) { return null; }
        return Max(root , max);
    }
    TreeNode Max(TreeNode root , TreeNode max){
        if (root == null) { return max; }
        if (root.val > max.val) {  max = root; }
        TreeNode left = Max(root.left , max);
        TreeNode right = Max(root.right , max);
        return  left.val > right.val ? left : right;
    }
}
```

### 6.二叉树的路径和<br/>
描述
给定一个二叉树，找出所有路径中各节点相加总和等于给定 目标值 的路径。
一个有效的路径，指的是从根节点到叶节点的路径。
``` java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */
public class Solution {
    /**
     * @param root the root of binary tree
     * @param target an integer
     * @return all valid paths
     */
    List<Integer> temp = new ArrayList<Integer>();
    List<List<Integer>> result = new ArrayList<List<Integer>>();
    public List<List<Integer>> binaryTreePathSum(TreeNode root, int target) {
        // Write your code here
         if(root == null) return result;
         temp.add(root.val);
         target -= root.val;
         if(target == 0 && root.left == null && root.right == null) result.add(new ArrayList<Integer>(temp));
            binaryTreePathSum(root.left, target);
            binaryTreePathSum(root.right,target);
            temp.remove(temp.size()-1);
            return result;
    }
}
```
### 7.删除链表中的元素
删除链表中等于给定值val的所有节点。
样例
给出链表 1->2->3->3->4->5->3, 和 val = 3, 你需要返回删除3之后的链表：1->2->4->5。
``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    /**
     * @param head a ListNode
     * @param val an integer
     * @return a ListNode
     */
    public ListNode removeElements(ListNode head, int val) {
        // Write your code here
        if (head == null){
            return null;
        }
        if (head.val == val){
            return removeElements(head.next, val);
        }
        if (head != null){
            ListNode last = head;
            ListNode next = head.next;
            while (next != null){
                if (next.val == val){
                  last.next = next.next;
                  next = next.next;
                } else {
                  last = last.next;
                  next = next.next;
                }
            }
        }
        // if(head.val == val){
        //     return head.next;
        // }
        return head;
    }
}
```
8.Same Tree
描述
检查两棵二叉树是否等价。等价的意思是说，首先两棵二叉树必须拥有相同的结构，并且每个对应位置上的节点上的数都相等。
``` java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */


public class Solution {
    /*
     * @param a: the root of binary tree a.
     * @param b: the root of binary tree b.
     * @return: true if they are identical, or false.
     */
    public boolean isIdentical(TreeNode a, TreeNode b) {
        // write your code here
        if(a==null && b==null) return true;
        if((a==null&&b!=null)||(a!=null&&b==null)) return false;
        if(a.val==b.val){
            return isIdentical(a.left,b.left) && isIdentical(a.right,b.right);
        }
        return false;
    }
}
```
### 10.单例 
``` java
class Solution {
    /**
     * @return: The same instance of this class every time
     */
    private static volatile Solution instance = null;  
    public static Solution getInstance() {
        // write your code here
         if(instance == null)  
            instance = new Solution();  
        return instance;  
    }
};
```

### 11.二分法排序
``` java
package sort;
//二分法排序
public class Dichotomy {
	  public static void main(String[] args) {
		          int[] a={49,38,65,97,176,213,227,49,78,34,12,164,11,18,1};
		          System.out.println("排序之前：");
		           for (int i = 0; i < a.length; i++) {
		               System.out.print(a[i]+" ");
		           }
		          //二分插入排序
		          sort(a);
		          System.out.println();
		          System.out.println("排序之后：");
		          for (int i = 0; i < a.length; i++) {
		              System.out.print(a[i]+" ");
		          }
		      }
		  
		      private static void sort(int[] a) {
		          for (int i = 0; i < a.length; i++) {
		              int temp = a[i];
		              int left = 0;
		              int right = i-1;
		              int mid = 0;
		             while(left<=right){
		                  mid = (left+right)/2;
		                  if(temp<a[mid]){
		                      right = mid-1;
		                  }else{
		                      left = mid+1;
		                  }
		              }
		              for (int j = i-1; j >= left; j--) {
		                  a[j+1] = a[j];
		              }
		              if(left != i){
		                  a[left] = temp;
		            }
		    }
     }
}
```
### 12.直接插入排序(稳定的)
``` java
package sort;
//直接插入排序(稳定的)
public class DirectInsertionSort {
	  public static void main(String[] args) {
		        int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1};
		         System.out.println("排序之前：");
		           for (int i = 0; i < a.length; i++) {
		              System.out.print(a[i]+" ");
		         }
		          //直接插入排序
		          for (int i = 1; i < a.length; i++) {
		             //待插入元素
		              int temp = a[i];
		              System.out.println("a[i]="+a[i]);
		              int j;
		              /*for (j = i-1; j>=0 && a[j]>temp; j--) {
		                  //将大于temp的往后移动一位
		                 a[j+1] = a[j];
		             }*/
		             for (j = i-1; j>=0; j--) {
		                  //将大于temp的往后移动一位
		                  if(a[j]>temp){
		                     a[j+1] = a[j];
		                 }else{
		                     break;
		                  }
		             }
		              a[j+1] = temp;
		              System.out.println("a[j+1]"+a[j+1]+"  "+(j+1));
		          }
		          System.out.println();
		          System.out.println("排序之后：");
		          for (int i = 0; i < a.length; i++) {
		              System.out.print(a[i]+" ");
		         }
		     }
		 }
```
### 13.堆排序
``` java
package sort;

import java.util.Arrays;
          //堆排序
	public class HeapSort {
		public static void main(String[] args) {
			int[] a = { 49, 38, 65, 97, 76, 13, 27, 49, 78, 34, 12, 64 };
			int arrayLength = a.length;
			// 循环建堆
			for (int i = 0; i < arrayLength - 1; i++) {
				// 建堆
				buildMaxHeap(a, arrayLength - 1 - i);
				// 交换堆顶和最后一个元素
				swap(a, 0, arrayLength - 1 - i);
				System.out.println(Arrays.toString(a));
			}
		}

		// 对data数组从0到lastIndex建大顶堆
		public static void buildMaxHeap(int[] data, int lastIndex) {
			// 从lastIndex处节点（最后一个节点）的父节点开始
			for (int i = (lastIndex - 1) / 2; i >= 0; i--) {
				// k保存正在判断的节点
				int k = i;
				// 如果当前k节点的子节点存在
				while (k * 2 + 1 <= lastIndex) {
					// k节点的左子节点的索引
					int biggerIndex = 2 * k + 1;
					// 如果biggerIndex小于lastIndex，即biggerIndex+1代表的k节点的右子节点存在
					if (biggerIndex < lastIndex) {
						// 若果右子节点的值较大
						if (data[biggerIndex] < data[biggerIndex + 1]) {
							// biggerIndex总是记录较大子节点的索引
							biggerIndex++;
						}
					}
					// 如果k节点的值小于其较大的子节点的值
					if (data[k] < data[biggerIndex]) {
						// 交换他们
						swap(data, k, biggerIndex);
						// 将biggerIndex赋予k，开始while循环的下一次循环，重新保证k节点的值大于其左右子节点的值
						k = biggerIndex;
					} else {
						break;
					}
				}
			}
		}

		// 交换
		private static void swap(int[] data, int i, int j) {
			int tmp = data[i];
			data[i] = data[j];
			data[j] = tmp;
		}
	}
```

### 14.希尔排序
``` java
package sort;
//希尔排序
public class HillSort {
	public static void main(String[] args) {
		          int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1};
		          System.out.println("排序之前：");
		         for (int i = 0; i < a.length; i++) {
		          System.out.print(a[i]+" ");
		        }
		        //希尔排序
		         int d = a.length;
		         while(true){
		             d = d / 2;
		             for(int x=0;x<d;x++){
		                 for(int i=x+d;i<a.length;i=i+d){
		                     int temp = a[i];
		                     int j;
		                    for(j=i-d;j>=0&&a[j]>temp;j=j-d){
		                        a[j+d] = a[j];
		                    }
		                     a[j+d] = temp;
		                 }
		             }
		             if(d == 1){
		                 break;
		             }
		         }
		         System.out.println();
		         System.out.println("排序之后：");
		         for (int i = 0; i < a.length; i++) {
		             System.out.print(a[i]+" ");
		         }
		    }
}
```
### 15.归并排序
``` java
package sort;
//归并排序
public class MergeSort {
	   public static void main(String[] args) {
		            int[] a={49,38,65,97,76,13,27,49,78,34,12,64,1,8};
		            System.out.println("排序之前：");
		           for (int i = 0; i < a.length; i++) {
		                System.out.print(a[i]+" ");
		           }
		           //归并排序
		          mergeSort(a,0,a.length-1);
		           System.out.println();
		           System.out.println("排序之后：");
		           for (int i = 0; i < a.length; i++) {
		                 System.out.print(a[i]+" ");
		           }
		       }
		      
		      private static void mergeSort(int[] a, int left, int right) {
		           if(left<right){
		               int middle = (left+right)/2;
		               //对左边进行递归
		               mergeSort(a, left, middle);
		               //对右边进行递归
		               mergeSort(a, middle+1, right);
		               //合并
		               merge(a,left,middle,right);
		           }
		       }
		   
		      private static void merge(int[] a, int left, int middle, int right) {
		          int[] tmpArr = new int[a.length];
		           int mid = middle+1; //右边的起始位置
		           int tmp = left;
		           int third = left;
		           while(left<=middle && mid<=right){
		               //从两个数组中选取较小的数放入中间数组
		               if(a[left]<=a[mid]){
		                   tmpArr[third++] = a[left++];
		               }else{
		                  tmpArr[third++] = a[mid++];
		               }
		           }
		          //将剩余的部分放入中间数组
		           while(left<=middle){
		               tmpArr[third++] = a[left++];
		           }
		          while(mid<=right){
		               tmpArr[third++] = a[mid++];
		           }
		           //将中间数组复制回原数组
		           while(tmp<=right){
		               a[tmp] = tmpArr[tmp++];
		           }
		       }
} 
```
### 16.快速排序
``` java
package sort;

//快速排序
public class QuickSort {
	public static void main(String[] args) {
		int[] a = { 49, 38, 65, 97, 76, 13, 27, 49, 78, 34, 12, 64, 1, 8 };
		System.out.println("排序之前：");
		for (int i = 0; i < a.length; i++) {
			System.out.print(a[i] + " ");
		}
		// 快速排序
		quick(a);
		System.out.println();
		System.out.println("排序之后：");
		for (int i = 0; i < a.length; i++) {
			System.out.print(a[i] + " ");
		}
	}

	private static void quick(int[] a) {
		if (a.length > 0) {
			quickSort(a, 0, a.length - 1);
		}
	}

	private static void quickSort(int[] a, int low, int high) {
		if (low < high) { // 如果不加这个判断递归会无法退出导致堆栈溢出异常
			int middle = getMiddle(a, low, high);
			quickSort(a, 0, middle - 1);
			quickSort(a, middle + 1, high);
		}
	}

	private static int getMiddle(int[] a, int low, int high) {
		int temp = a[low];// 基准元素
		while (low < high) {
			// 找到比基准元素小的元素位置
			while (low < high && a[high] >= temp) {
				high--;
			}
			a[low] = a[high];
			while (low < high && a[low] <= temp) {
				low++;
			}
			a[high] = a[low];
		}
		a[low] = temp;
		return low;
	}
}
```

### 17.基数排序，稳定
``` java
package sort;

import java.util.ArrayList;
import java.util.List;
//基数排序，稳定
public class RadixSorting {
	 public static void main(String[] args) {
		         int[] a={49,38,65,97,176,213,227,49,78,34,12,164,11,18,1};
		          System.out.println("排序之前：");
		        for (int i = 0; i < a.length; i++) {
		             System.out.print(a[i]+" ");
		         }
		         //基数排序
		         sort(a);
		       System.out.println();
		         System.out.println("排序之后：");
		         for (int i = 0; i < a.length; i++) {
		             System.out.print(a[i]+" ");
		         }
		     }
		 
		     private static void sort(int[] array) {
		         //找到最大数，确定要排序几趟
		         int max = 0;
		         for (int i = 0; i < array.length; i++) {
		            if(max<array[i]){
		                 max = array[i];
		             }
		         }
		         //判断位数
		         int times = 0;
		         while(max>0){
		            max = max/10;
		             times++;
		         }
		         //建立十个队列
		         List<ArrayList> queue = new ArrayList<ArrayList>();
		         for (int i = 0; i < 10; i++) {
		             ArrayList queue1 = new ArrayList();
		             queue.add(queue1);
		         }
		         //进行times次分配和收集
		        for (int i = 0; i < times; i++) {
		             //分配
		             for (int j = 0; j < array.length; j++) {
		                int x = array[j]%(int)Math.pow(10, i+1)/(int)Math.pow(10, i);
		                 ArrayList queue2 = queue.get(x);
		                 queue2.add(array[j]);
		                 queue.set(x,queue2);
		             }
		             //收集
		             int count = 0;
		             for (int j = 0; j < 10; j++) {
		                 while(queue.get(j).size()>0){
		                     ArrayList<Integer> queue3 = queue.get(j);
		                     array[count] = queue3.get(0);
		                    queue3.remove(0);
		                     count++;
		                 }
		             }
		         }
		     }
}
```
### 18.选择排序
``` java
package sort;
//选择排序
public class SelectionSort {
	public static void main(String[] args) {
		int[] a = { 49, 38, 65, 97, 76, 13, 27, 49, 78, 34, 12, 64, 1, 8 };
		System.out.println("排序之前：");
		for (int i = 0; i < a.length; i++) {
			System.out.print(a[i] + " ");
		}
		// 简单的选择排序
		for (int i = 0; i < a.length; i++) {
			int min = a[i];
			int n = i; // 最小数的索引
			for (int j = i + 1; j < a.length; j++) {
				if (a[j] < min) { // 找出最小的数
					min = a[j];
					n = j;
				}
			}
			a[n] = a[i];
			a[i] = min;

		}
		System.out.println();
		System.out.println("排序之后：");
		for (int i = 0; i < a.length; i++) {
			System.out.print(a[i] + " ");
		}
	}
}
```
### 19.各算法空间复杂度和时间复杂度的对比
![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018070202.png)
![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018070201.png)

## <div class="note default"><p>数据库篇</p></div>
### 1. 主键 超键 候选键 外键
- 主 键：
数据库表中对储存数据对象予以唯一和完整标识的数据列或属性的组合。一个数据列只能有一个主键，且主键的取值不能缺失，即不能为空值（Null）。
- 超 键：
在关系中能唯一标识元组的属性集称为关系模式的超键。一个属性可以为作为一个超键，多个属性组合在一起也可以作为一个超键。超键包含候选键和主键。
- 候选键：
是最小超键，即没有冗余元素的超键。
- 外 键：
在一个表中存在的另一个表的主键称此表的外键。

### 2.数据库事务的四个特性及含义
数据库事务transanction正确执行的四个基本要素。ACID,原子性(Atomicity)、一致性(Correspondence)、隔离性(Isolation)、持久性(Durability)。 

- 原子性:整个事务中的所有操作，要么全部完成，要么全部不完成，不可能停滞在中间某个环节。事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样。 
- 一致性:在事务开始之前和事务结束以后，数据库的完整性约束没有被破坏。 
- 隔离性:隔离状态执行事务，使它们好像是系统在给定时间内执行的唯一操作。如果有两个事务，运行在相同的时间内，执行 相同的功能，事务的隔离性将确保每一事务在系统中认为只有该事务在使用系统。这种属性有时称为串行化，为了防止事务操作间的混淆，必须串行化或序列化请 求，使得在同一时间仅有一个请求用于同一数据。 
- 持久性:在事务完成以后，该事务所对数据库所作的更改便持久的保存在数据库之中，并不会被回滚。

### 3.索引的工作原理及其种类
数据库索引，是数据库管理系统中一个排序的数据结构，以协助快速查询、更新数据库表中数据。索引的实现通常使用B树及其变种B+树。<br/>
在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法。这种数据结构，就是索引。<br/>
为表设置索引要付出代价的：一是增加了数据库的存储空间，二是在插入和修改数据时要花费较多的时间(因为索引也要随之变动)。
![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018070203.png)
上图展示了一种可能的索引方式。左边是数据表，一共有两列七条记录，最左边的是数据记录的物理地址（注意逻辑上相邻的记录在磁盘上也并不是一定物理相邻的）。为了加快Col2的查找，可以维护一个右边所示的二叉查找树，每个节点分别包含索引键值和一个指向对应数据记录物理地址的指针，这样就可以运用二叉查找在O(log2n)的复杂度内获取到相应数据。

创建索引可以大大提高系统的性能。

- 第一，通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性。

- 第二，可以大大加快数据的检索速度，这也是创建索引的最主要的原因。

- 第三，可以加速表和表之间的连接，特别是在实现数据的参考完整性方面特别有意义。

- 第四，在使用分组和排序子句进行数据检索时，同样可以显著减少查询中分组和排序的时间。

- 第五，通过使用索引，可以在查询的过程中，使用优化隐藏器，提高系统的性能。

也许会有人要问：增加索引有如此多的优点，为什么不对表中的每一个列创建一个索引呢？因为，增加索引也有许多不利的方面。

- 第一，创建索引和维护索引要耗费时间，这种时间随着数据量的增加而增加。

- 第二，索引需要占物理空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，如果要建立聚簇索引，那么需要的空间就会更大。

- 第三，当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，这样就降低了数据的维护速度。

索引是建立在数据库表中的某些列的上面。在创建索引的时候，应该考虑在哪些列上可以创建索引，在哪些列上不能创建索引。一般来说，应该在这些列上创建索引：在经常需要搜索的列上，可以加快搜索的速度；在作为主键的列上，强制该列的唯一性和组织表中数据的排列结构；在经常用在连接的列上，这些列主要是一些外键，可以加快连接的速度；在经常需要根据范围进行搜索的列上创建索引，因为索引已经排序，其指定的范围是连续的；在经常需要排序的列上创建索引，因为索引已经排序，这样查询可以利用索引的排序，加快排序查询时间；在经常使用在WHERE子句中的列上面创建索引，加快条件的判断速度。

同样，对于有些列不应该创建索引。一般来说，不应该创建索引的的这些列具有下列特点：

- 第一，对于那些在查询中很少使用或者参考的列不应该创建索引。这是因为，既然这些列很少使用到，因此有索引或者无索引，并不能提高查询速度。相反，由于增加了索引，反而降低了系统的维护速度和增大了空间需求。

- 第二，对于那些只有很少数据值的列也不应该增加索引。这是因为，由于这些列的取值很少，例如人事表的性别列，在查询的结果中，结果集的数据行占了表中数据行的很大比例，即需要在表中搜索的数据行的比例很大。增加索引，并不能明显加快检索速度。

- 第三，对于那些定义为text, image和bit数据类型的列不应该增加索引。这是因为，这些列的数据量要么相当大，要么取值很少。

- 第四，当修改性能远远大于检索性能时，不应该创建索引。这是因为，修改性能和检索性能是互相矛盾的。当增加索引时，会提高检索性能，但是会降低修改性能。当减少索引时，会提高修改性能，降低检索性能。因此，当修改性能远远大于检索性能时，不应该创建索引。

根据数据库的功能，可以在数据库设计器中创建三种索引：唯一索引、主键索引和聚集索引。

唯一索引 
唯一索引是不允许其中任何两行具有相同索引值的索引。

当现有数据中存在重复的键值时，大多数数据库不允许将新创建的唯一索引与表一起保存。数据库还可能防止添加将在表中创建重复键值的新数据。例如，如果在employee表中职员的姓(lname)上创建了唯一索引，则任何两个员工都不能同姓。 主键索引 数据库表经常有一列或列组合，其值唯一标识表中的每一行。该列称为表的主键。 在数据库关系图中为表定义主键将自动创建主键索引，主键索引是唯一索引的特定类型。该索引要求主键中的每个值都唯一。当在查询中使用主键索引时，它还允许对数据的快速访问。 聚集索引 在聚集索引中，表中行的物理顺序与键值的逻辑（索引）顺序相同。一个表只能包含一个聚集索引。 
如果某索引不是聚集索引，则表中行的物理顺序与键值的逻辑顺序不匹配。与非聚集索引相比，聚集索引通常提供更快的数据访问速度。

局部性原理与磁盘预读 
由于存储介质的特性，磁盘本身存取就比主存慢很多，再加上机械运动耗费，磁盘的存取速度往往是主存的几百分分之一，因此为了提高效率，要尽量减少磁盘I/O。为了达到这个目的，磁盘往往不是严格按需读取，而是每次都会预读，即使只需要一个字节，磁盘也会从这个位置开始，顺序向后读取一定长度的数据放入内存。这样做的理论依据是计算机科学中著名的局部性原理：当一个数据被用到时，其附近的数据也通常会马上被使用。程序运行期间所需要的数据通常比较集中。

由于磁盘顺序读取的效率很高（不需要寻道时间，只需很少的旋转时间），因此对于具有局部性的程序来说，预读可以提高I/O效率。

预读的长度一般为页（page）的整倍数。页是计算机管理存储器的逻辑块，硬件及操作系统往往将主存和磁盘存储区分割为连续的大小相等的块，每个存储块称为一页（在许多操作系统中，页得大小通常为4k），主存和磁盘以页为单位交换数据。当程序要读取的数据不在主存中时，会触发一个缺页异常，此时系统会向磁盘发出读盘信号，磁盘会找到数据的起始位置并向后连续读取一页或几页载入内存中，然后异常返回，程序继续运行。

B-/+Tree索引的性能分析 
到这里终于可以分析B-/+Tree索引的性能了。

上文说过一般使用磁盘I/O次数评价索引结构的优劣。先从B-Tree分析，根据B-Tree的定义，可知检索一次最多需要访问h个节点。数据库系统的设计者巧妙利用了磁盘预读原理，将一个节点的大小设为等于一个页，这样每个节点只需要一次I/O就可以完全载入。为了达到这个目的，在实际实现B-Tree还需要使用如下技巧：

每次新建节点时，直接申请一个页的空间，这样就保证一个节点物理上也存储在一个页里，加之计算机存储分配都是按页对齐的，就实现了一个node只需一次I/O。

B-Tree中一次检索最多需要h-1次I/O（根节点常驻内存），渐进复杂度为O(h)=O(logdN)。一般实际应用中，出度d是非常大的数字，通常超过100，因此h非常小（通常不超过3）。

而红黑树这种结构，h明显要深的多。由于逻辑上很近的节点（父子）物理上可能很远，无法利用局部性，所以红黑树的I/O渐进复杂度也为O(h)，效率明显比B-Tree差很多。

综上所述，用B-Tree作为索引结构效率是非常高的。

### 4.数据库范式
- 1 第一范式（1NF）

在任何一个关系数据库中，第一范式（1NF）是对关系模式的基本要求，不满足第一范式（1NF）的数据库就不是关系数据库。 
所谓第一范式（1NF）是指数据库表的每一列都是不可分割的基本数据项，同一列中不能有多个值，即实体中的某个属性不能有多个值或者不能有重复的属性。如果出现重复的属性，就可能需要定义一个新的实体，新的实体由重复的属性构成，新实体与原实体之间为一对多关系。在第一范式（1NF）中表的每一行只包含一个实例的信息。简而言之，第一范式就是无重复的列。

- 2 第二范式（2NF）

第二范式（2NF）是在第一范式（1NF）的基础上建立起来的，即满足第二范式（2NF）必须先满足第一范式（1NF）。第二范式（2NF）要求数据库表中的每个实例或行必须可以被惟一地区分。为实现区分通常需要为表加上一个列，以存储各个实例的惟一标识。这个惟一属性列被称为主关键字或主键、主码。 
第二范式（2NF）要求实体的属性完全依赖于主关键字。所谓完全依赖是指不能存在仅依赖主关键字一部分的属性，如果存在，那么这个属性和主关键字的这一部分应该分离出来形成一个新的实体，新实体与原实体之间是一对多的关系。为实现区分通常需要为表加上一个列，以存储各个实例的惟一标识。简而言之，第二范式就是非主属性非部分依赖于主关键字。

- 3 第三范式（3NF）

满足第三范式（3NF）必须先满足第二范式（2NF）。简而言之，第三范式（3NF）要求一个数据库表中不包含已在其它表中已包含的非主关键字信息。例如，存在一个部门信息表，其中每个部门有部门编号（dept_id）、部门名称、部门简介等信息。那么在员工信息表中列出部门编号后就不能再将部门名称、部门简介等与部门有关的信息再加入员工信息表中。如果不存在部门信息表，则根据第三范式（3NF）也应该构建它，否则就会有大量的数据冗余。简而言之，第三范式就是属性不依赖于其它非主属性。（我的理解是消除冗余）
<br/>
### 5.数据库优化的思路
1.SQL语句优化

- 1.应尽量避免在 where 子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描。 
- 2.应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描，如： 
select id from t where num is null 
可以在num上设置默认值0，确保表中num列没有null值，然后这样查询： 
select id from t where num=0 
- 3.很多时候用 exists 代替 in 是一个好的选择 
- 4.用Where子句替换HAVING 子句 因为HAVING 只会在检索出所有记录之后才对结果集进行过滤 

2.索引优化<br/>

3.数据库结构优化

- 1.范式优化： 比如消除冗余（节省空间。。） 
- 2.反范式优化：比如适当加冗余等（减少join） 
- 3.拆分表： 分区将数据在物理上分隔开，不同分区的数据可以制定保存在处于不同磁盘上的数据文件里。这样，当对这个表进行查询时，只需要在表分区中进行扫描，而不必进行全表扫描，明显缩短了查询时间，另外处于不同磁盘的分区也将对这个表的数据传输分散在不同的磁盘I/O，一个精心设置的分区可以将数据传输对磁盘I/O竞争均匀地分散开。对数据量大的时时表可采取此方法。可按月自动建表分区。 
- 4.拆分其实又分垂直拆分和水平拆分： 案例： 简单购物系统暂设涉及如下表：<br/> 1.产品表（数据量10w，稳定） <br/> 2.订单表（数据量200w，且有增长趋势） <br/> 3.用户表 （数据量100w，且有增长趋势） 以mysql为例讲述下水平拆分和垂直拆分，mysql能容忍的数量级在百万静态数据可以到千万 垂直拆分： 解决问题：表与表之间的io竞争 不解决问题：单表中数据量增长出现的压力 方案： 把产品表和用户表放到一个server上 订单表单独放到一个server上 水平拆分： 解决问题：单表中数据量增长出现的压力 不解决问题：表与表之间的io争夺 
方案： 用户表通过性别拆分为男用户表和女用户表 订单表通过已完成和完成中拆分为已完成订单和未完成订单 产品表 未完成订单放一个server上 已完成订单表盒男用户表放一个server上 女用户表放一个server上。

4.服务器硬件优化
<br/>
**参考链接**：
1.[Java面试题全集（上）](http://www.importnew.com/22083.html)
2.[Java_框架面试题](https://www.cnblogs.com/xuzimian/p/8603060.html)
3.[java面试之数据库面试知识点](http://java%E9%9D%A2%E8%AF%95%E4%B9%8B%E6%95%B0%E6%8D%AE%E5%BA%93%E9%9D%A2%E8%AF%95%E7%9F%A5%E8%AF%86%E7%82%B9)
