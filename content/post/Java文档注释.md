---
title: Java文档注释
date: 2018-06-13 16:58:31
categories: ["Java"]
tags: ["文档注释"]
featured_image: https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/bing_photos/20180613.jpg
---

<p class="description">Have the courage to follow your heart and intuition. They somehow already know what you truly want to become.</p>
<!-- more -->
### 1 基本概念介绍
如果要想是一个API真正可用，就必须为其编写文档。传统意义上的文档是手工生成的，所以保持文档与代码同步是一件很繁琐的事情。Java语言环境提供了一种被称为JavaDoc的实用工具，从而使这项任务变得很容易。JavaDoc利用特殊格式的文档注释（Documentation Comment，通常被写作doc comment）,根据源代码自动生成API文档。
#### 1.1 注释方式
Java 支持三种注释方式。前两种分别是 // 和 /* */，第三种被称作说明注释，它以 /** 开始，以 */结束。
说明注释允许你在程序中嵌入关于程序的信息。你可以使用 javadoc 工具软件来生成信息，并输出到HTML文件中。
说明注释，使你更加方便的记录你的程序信息。
#### 1.2 javadoc 标签
| 标签     | 描述 | 示例   |
| :---: | :---: | :---: |
| @author | 标识一个类的作者 |  @author description    |
| @deprecated	    | 指名一个过期的类或成员	  |  @deprecated description
   |
| {@docRoot}     | 指明当前文档根目录的路径	   |  Directory Path  |
| @exception    | 标志一个类抛出的异常	   |  @exception exception-name explanation
  |
| {@inheritDoc}    | 从直接父类继承的注释	   |  Inherits a comment from the immediate surperclass.
  |
| {@link}   | 插入一个到另一个主题的链接	   |  	{@link name text}  |
| {@linkplain}   | 插入一个到另一个主题的链接，但是该链接显示纯文本字体	   |  	Inserts an in-line link to another topic.  |
| @param  | 说明一个方法的参数		   |  	@param parameter-name explanation
  |
| @return  | 说明返回值类型	   |  	@return explanation
  |
| @see	   | 指定一个到另一个主题的链接	   |  	@see anchor  |
| @serial	   | 指定一个到另一个主题的链接	   |  	@see anchor  |
| @serialData		   | 说明通过writeObject( ) 和 writeExternal( )方法写的数据	   |  	@serialData description  |
| @serialField		   | 	说明一个ObjectStreamField组件	   |  		@serialField name type description  |
| @since	   | 标记当引入一个特定的变化时		   |  	@since release  |
| @throws	   | 和 @exception标签一样.	   |  	The @throws tag has the same meaning as the @exception tag.  |
| {@value}	   | 显示常量的值，该常量必须是static属性。		   |  	Displays the value of a constant, which must be a static field.  |
| @version	   | 指定类的版本	   |  	@version info  |

#### 1.3 javadoc 输出什么

javadoc 工具将你 Java 程序的源代码作为输入，输出一些包含你程序注释的HTML文件。
每一个类的信息将在独自的HTML文件里。javadoc 也可以输出继承的树形结构和索引。
由于 javadoc 的实现不同，工作也可能不同，你需要检查你的 Java 开发系统的版本等细节，选择合适的 Javadoc 版本。

下面是一个使用说明注释的简单实例。注意每一个注释都在它描述的项目的前面。
在经过 javadoc 处理之后，SquareNum 类的注释将在 SquareNum.html 中找到。
``` java
import java.io.*;
 
/**
* 这个类演示了文档注释
* @author sdw
* @version 1.2
*/
public class SquareNum {
   /**
   * This method returns the square of num.
   * This is a multiline description. You can use
   * as many lines as you like.
   * @param num The value to be squared.
   * @return num squared.
   */
   public double square(double num) {
      return num * num;
   }
   /**
   * This method inputs a number from the user.
   * @return The value input as a double.
   * @exception IOException On input error.
   * @see IOException
   */
   public double getNumber() throws IOException {
      InputStreamReader isr = new InputStreamReader(System.in);
      BufferedReader inData = new BufferedReader(isr);
      String str;
      str = inData.readLine();
      return (new Double(str)).doubleValue();
   }
   /**
   * This method demonstrates square().
   * @param args Unused.
   * @return Nothing.
   * @exception IOException On input error.
   * @see IOException
   */
   public static void main(String args[]) throws IOException
   {
      SquareNum ob = new SquareNum();
      double val;
      System.out.println("Enter value to be squared: ");
      val = ob.getNumber();
      val = ob.square(val);
      System.out.println("Squared value is " + val);
   }
}
```

>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018061301.png)

测试时发现一个问题,main函数不能加return注释，去掉就行了

>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018061302.png)

生产的文件

>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018061303.png)
>![image](https://sdw-1254060699.cos.ap-chengdu.myqcloud.com/2018061304.png)



### 2 注意事项
Java 1.5 发行版本中增加的三个特性在文档注释时需要特别小心:泛型、枚举、和注解。
####2.1 泛型
为泛型或者方法编写文档时，确保要在文档中说明所有类型的参数
``` java
/**
 *@param <K> the type of keys maintained by this map
 *@param <V> the type of mapped values
```
#### 2.2 枚举
为枚举类型编写文档时，要确保在文档中说明常量及类型，还有任何公有的方法。
注意，如果文档注释很短，可以将整个注释放在一行上：
``` java
public enum OrchestraSection{
    /** Woodwinds,such as flute,clarinet and oboe. */
    WOODWIND,
    /** Brass instruments,such as french horn and trumpet
    BRASS,
    /** Percussion instruments, such as timpani and cymbals */
    PERCUSSION,
    /** Stringded instruments,such as violin and cello */
    STRING
}
```
#### 2.3 注解
为注解类型编写文档时，要确保在文档中说明所有成员，以及类型本身。带有名词短语的文档成员，就像是域一样，对于该类型的描述，要使用一个动词短语，说明这种类型的注解时它表示的什么意思。
``` java
/**
 *@Retention(RetentionPolicy.RUNTIME)
 *@Target(ElementType.METHOD)
public @interface ExceptionTest {
    Class<? extends Exception> value();
```

总结：虽然为所有导出的API元素提供文档注释是必要的，但是这样做并非永远就足够了。对于由多个相互关联的类组成的复杂API,通常有必要用一个外部文档来描述该API的总体结构。对于文档注释进行补充。如果有这样的文档，相关的类或者包文档注释就应该包含一个对这个外部文档的链接。
对于所有可导出的API元素来说，使用文档注释应该是被看作是强制性的，要采用一致的风格来遵循标准的约定。在文档注释内部出现任何HTML标签都是允许的，但是HTML元字符必须经过转义。






