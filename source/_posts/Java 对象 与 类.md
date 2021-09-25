---
title: Java 对象 与 类
tags:
- Java
- Java核心技术卷1读书笔记
categories: 编程
---

## 类

类（class) 是构造对象的模板或蓝图。我们可以将类想象成制作小甜饼的切割机，将对 象想象为小甜饼。由类构造（construct) 对象的过程称为创建类的实例 （instance). 

**封装**（encapsulation, 有时称为数据隐藏）是与对象有关的一个重要概念。从形式上看， 封装不过是将数据和行为组合在一个包中， 并对对象的使用者隐藏了数据的实现方式。对象 中的数据称为实例域（ instance field), 操纵数据的过程称为方法（method )。对于每个特定的 类实例（对象）都有一组特定的实例域值。这些值的集合就是这个对象的当前状态（state)。 无论何时，只要向对象发送一个消息，它的状态就有可能发生改变。

实现封装的关键在于绝对不能让类中的方法直接地访问其他类的实例域。程序仅通过对 象的方法与对象数据进行交互。封装给对象赋予了“ 黑盒” 特征， 这是提高重用性和可靠性 的关键。 

## 对 象

要想使用 OOP, —定要清楚对象的三个主要特性： 

•对象的行为（behavior)— —可以对对象施加哪些操作，或可以对对象施加哪些方法？

 •对象的状态（state)— —当施加那些方法时，对象如何响应？

 •对象标识（identity)— —如何辨别具有相同行为与状态的不同对象？ 

##  使用预定义类

### 对象与对象变量

要想使用对象，就必须首先构造对象， 并指定其初始状态。然后，对对象应用方法。 在 Java 程序设计语言中， 使用构造器（constructor) 构造新实例。构造器是一种特殊的方法， 用来构造并初始化对象。

在对象与对象变量之间存在着一个重要的区别。例如， 语句

 Date deadline; // deadline doesn't refer to any object

定义了一个对象变量 deadline, 它可以引用 Date 类型的对象。但是，一定要认识到： 变量 deadline 不是一个对象， 实际上也没有引用对象。此时，不能将任何 Date 方法应用于这个变 量上。

语句 s = deadline.toString(); // not yet 

将产生编译错误。 必须首先初始化变量 deadline, 这里有两个选择。当然，可以用新构造的对象初始化这 个变量： 

deadline = new Date(); 

也让这个变量引用一个已存在的对象： 

deadline = birthday; 

现在，这两个变量引用同一个对象

**一定要认识到： 一个对象变量并没有实际包含一个对象，而仅仅引用一个对象。** 

在 Java 中，任何对象变量的值都是对存储在另外一个地方的一个对象的引用。new 操作 符的返回值也是一个引用。

只 访 问 对 象 而 不 修 改 对 象 的 方 法 有 时 称 为 访 问 器 方 法

### Java 类库中的 LocalDate 类

类库设计者决定将保存时间与给时间点命名分开。所以标准 Java 类库分别包含了两个类：一个是用来表示时间点的 Date 类；另一个是用来表示大家熟悉的日历表示法的 LocalDate 类。

用 LocalDate 类打印一个日历：

 ![calendar](https://github.com/Misakasister/misakasister.github.io/blob/master/images/java_core_technonlgy_1/oop/calendar.png?raw=true)

 ```java
public class Calendar {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		LocalDate now = LocalDate.now();//当前时间
		System.out.println("Mon\tTue\tWed\tThu\tFri\tSat\tSun");
		int month = now.getMonthValue();
		LocalDate temp = now.minusDays(now.getDayOfMonth()-1);//本月第一天
		//本月第一天对应的星期几
		int nowMonthFristWeek = temp.getDayOfWeek().getValue();
		//本月第一天前都是空格
		for(int i = 1; i < nowMonthFristWeek; i++) {
			System.out.print("\t");
		}
		//输出日历主体部分->只要在本月就打印
		while(temp.getMonthValue()==month) {
			if(temp.getDayOfMonth()==now.getDayOfMonth()) {
				System.out.printf("%d*\t",temp.getDayOfMonth());
			}else {
				System.out.printf("%d\t",temp.getDayOfMonth());
			}
			if(temp.getDayOfWeek().getValue()==7) {
				System.out.println();
			}
			temp = temp.plusDays(1);
		}
	}
}
 ```

## final 实例域

final 修饰符大都应用于基本（primitive) 类型域，或不可变（immutable) 类的域（如果类 中的每个方法都不会改变其对象， 这种类就是不可变的类。例如，String类就是一个不可变 的类)。 

```java
private final String name;//这个值不会被修改。即没有setName方法
```

setName()方法比把name设置为public的好处是可以在setName()中添加对name限定条件。

对于可变的类， 使用 final 修饰符可能会对读者造成混乱。例如：

 private final StringBuiIcier evaluations;
在 构造器中会初始化为 

evaluations = new StringBuilder();

 final 关键字只是表示存储在 evaluations 变量中的对象引用不会再指示其他 StringBuilder 对象。不过这个对象可以更改： 

## 静态域与静态方法

### 静态域

如果将域定义为 static, 每个类中只有一个这样的域。而每一个对象对于所有的实例域 却都有自己的一份拷贝。

```java
class Employee { 
private static int nextld = 1 ;
private int id;
}
```

现在， 每一个雇员对象都有一个自己的 id 域， 但这个类的所有实例将共享一个 nextId 域。换句话说， 如果有 1000 个 Employee类的对象， 则有 1000 个实例域 id。但是，只有一 个静态域 nextld。即使没有一个雇员对象， 静态域 nextld 也存在。它属于类，而不属于任何 独立的对象。 

### 静态常量

 在 Math类中定义了一个 静态常量： 

```java
public class Hath {
public static final double PI=3.14159265358979323846;
} 
```

在程序中，可以采用 Math.PI 的形式获得这个常量。

如果关键字 static 被省略， PI 就变成了 Math 类的一个实例域。需要通过 Math类的对象 访问 PI，并且每一个 Math 对象都有它自己的一份 PI 拷贝。

### 静态方法

静态方法是一种不能向对象实施操作的方法。例如， Math的 pow 方法就是一静态方法。表达式 

Math.pow(x, a) 

计算幂x^a。在运算时，不使用任何Math对象。换句话说，没有隐式参数。

可以认为静态方法是没有this参数的方法。

静态方法不能访问对象的实例域，可以访问自身类的静态域。

在下面两种情况下使用静态方法：

•一个方法不需要访问对象状态，其所需参数都是通过显式参数提供（例如:Math.pow) 

•一个方法只需要访问类的静态域

### 工厂方法

静态方法还有另外一种常见的用途。类似 LocalDate 和 NumberFormat 的类使用静态工厂方法 (factory method） 来构造对象。

```java
NumberFormat currencyFormatter = NumberFormat.getCurrencylnstance(); 
NumberFormat percentFormatter = NumberFormat.getPercentlnstance()； 
double x = 0.1 ; 
System.out.println(currencyFormatter.format(x)); // prints SO.10 
System.out.println(percentFomatter.format(x)); // prints 10%
```

为什么 NumberFormat 类不利用构造器完成这些操作呢？ 这主要有两个原因： 

•无法命名构造器。构造器的名字必须与类名相同。但是， 这里希望将得到的货币实例 和百分比实例采用不用的名字。 

•当使用构造器时，无法改变所构造的对象类型。而 Factory方法将返回一个 DecimalFormat 类对象，这是 NumberFormat 的子类。

### main 方法

main方法是一个静态方法。

main方法不对任何对象进行操作。事实上，在启动程序时还没有任何一个对象。静态的 main方法将执行并创建程序所需要的对象。 

 每一个类可以有一个 main 方法。这是一个常用于对类进行单元测试的技巧。

如果想单独测试某个类，则执行：

`java 测试类名`

如果A类有main方法且是一个更大应用程序B类的一部分，就可以使用：

`java B`

这样A类的main方法就会被忽略。

## 对象构造

###  重载

如果多个方法（比如， StringBuilder 构造器方法）有相同的名字、不同的参数，便产生了重载。

不能有两个名字相同、 参数类型也相 同却返回不同类型值的方法。

###  默认域初始化

如果在构造器中没有显式地给域赋予初值，那么就会被自动地赋为默认值： 数值为 0、 布尔值为 false、 对象引用为null。

这是域与局部变量的主要不同点。必须明确地初始化方法中的局部变量。 但是， 如果没有初始化类中的域， 将会被自动初始化为默认值（0、false 或 null)。 

###  无参数的构造器

很多类都包含一个无参数的构造函数，对象由无参数构造函数创建时， 其状态会设置为 适当的默认值。 

```java
public Employee0 { 
    name =""; 
    salary = 0; 
    hireDay = LocalDate,now(); 
} 
```

如果希望所有域被赋
予默认值， 可以采用下列格式：

```java
public ClassName(){}
```

### 显示域初始化

可以在类定义中， 直接将一个值赋给任何域。例如： 

```java
class Employee{
    private String name = "";
}
```

在执行构造器之前，先执行赋值操作。当一个类的所有构造器都希望把相同的值赋予某 个特定的实例域时，这种方式特别有用。

 ### 调用另一个构造器

关键字 this 引用方法的隐式参数。然而，这个关键字还有另外一个含义。 如果构造器的第一个语句形如 this(...)， 这个构造器将**调用同一个类的另一个构造器**。下 面是一个典型的例子： 

```java
public Employee(double s) { 
    // calls Employee(String, double) 
    this("Employee #" + nextld, s); 
    nextld++; } 
```

当调用 new Employee(60000) 时， Employee(double) 构造器将调用 Employee(String，double) 构造器。 

### 初始化块

前面已经讲过两种初始化数据域的方法： 

•在构造器中设置值 

•在声明中赋值 

实际上，Java 还有第三种机制， 称为初始化块（initialization block)。在一个类的声明中， 可以包含多个代码块。只要构造类的对象，这些块就会被执行。例如：

```java 
public Employee{
    private static int nextId;
    private int id;
    {
        id=nextId;
        nextId++;
    }
}
```

在这个示例中，无论使用哪个构造器构造对象，id 域都在对象初始化块中被初始化。首 先运行初始化块，然后才运行构造器的主体部分。 

下面是调 用构造器的具体处理步骤： 

1 ) 所有数据域被初始化为默认值（0、false 或 null)。 

2 ) 按照在类声明中出现的次序， 依次执行所有域初始化语句和初始化块。
3 ) 如果构造器第一行调用了第二个构造器，则执行第二个构造器主体 

4 ) 执行这个构造器的主体. 

如果对类的静态域进行初始化的代码比较复杂，那么可以使用静态的初始化块。 

将代码放在一个块中，并标记关键字 static。

```java
static{
    //...
}
```

在**类第一次加载**的时候， 将会进行静态域的初始化。

### 对象析构与 finalize 方法

有些面向对象的程序设计语言，特别是 C++, 有显式的析构器方法，其中放置一些当对 象不再使用时需要执行的清理代码。在析构器中， 最常见的操作是回收分配给对象的存储空 间。由于 Java 有自动的垃圾回收器，不需要人工回收内存， 所以 Java 不支持析构器。

当然，某些对象使用了内存之外的其他资源， 例如，文件或使用了系统资源的另一个对 象的句柄。在这种情况下，当资源不再需要时， 将其回收和再利用将显得十分重要。

可以为任何一个类添加 finalize 方法。finalize 方法将在垃圾回收器清除对象之前调用。 在实际应用中，不要依赖于使用 finalize 方法回收任何短缺的资源， **这是因为很难知道这个 方法什么时候才能够调用。** 

如果某个资源需要在使用完毕后立刻被关闭， 那么就需要由人工来管理。对象用完时， 可以应用一个 close方法来完成相应的清理操作。

## 包

Java 允许使用包（package）将类组织起来。借助于包可以方便地组织自己的代码，并将 自己的代码与别人提供的代码库分开管理。 

使用包的主要原因是确保类名的唯一性。假如两个程序员不约而同地建立了 Employee 类。只要将这些类放置在不同的包中， 就不会产生冲突。事实上，为了保证包名的绝对 唯一性， Sun 公司建议将公司的因特网域名（这显然是独一无二的）以逆序的形式作为包 名，并且对于不同的项目使用不同的子包。

从编译器的角度来看， **嵌套的包之间没有任何关系**。例如，java.util 包与java.util.jar 包 毫无关系。每一个都拥有独立的类集合。

### 类的导入

 一个类可以使用所属包中的所有类， 以及其他包中的**公有类**（public class)。(一个包可以有很多公有类（.java 文件）)我们可以 采用两种方式访问另一个包中的公有类。第一种方式是在每个类名之前添加完整的包名。 例如：

`java.time.LocalDate today = java.time.LocalDate.now(); `

 这显然很繁琐。更简单且更常用的方式是使用 import 语句。import 语句是一种引用包含 在包中的类的简明描述。一旦使用了 import 语句，在使用类时，就不必写出包的全名了。 

java.time.* 的语法比较简单，对代码的大小也没有任何负面影响。当然， 如果能够明确地指出所导的类， 将会使代码的读者更加准确地知道加载了哪些类。 

但是， 需要注意的是， 只能使用星号（*) 导入一个包， 而不能使用 import java.\* 或 import java.\*.\*导入以 java 为前缀的所有包。 

----

要想使用Entry类，必须`import java.util.Map.Entry` 只有`import java.util.*`是不可以的,.*是代表导入该文件夹下的所有**类**，而`java.util`跟`java.util.Map` 不是同一个文件夹。 

从编译器的角度来看， **嵌套的包之间没有任何关系**。

---

在大多数情况下， 只导入所需的包， 并不必过多地理睬它们。但在发生命名冲突的时 候，就不能不注意包的名字了。例如，java.util 和java.sql 包都有日期（ Date) 类。如果在程 序中导入了这两个包。

在程序使用 Date 类的时候， 就会出现一个编译错误。

此时编译器无法确定程序使用的是哪一个 Date 类。可以采用增加一个特定的 import 语句来 解决这个问题： 

```java
import java.util.*;
import java.sql.*;
import java.util.Date;
```

如果这两个 Date 类都需要使用，又该怎么办呢？ 答案是，在每个类名的前面加上完整的包名。

### 静态导入

 import 语句不仅可以导入类，还增加了导入静态方法和静态域的功能。 

例如，如果在源文件的顶部， 添加一条指令： 

`import static java.lang.System.*;`

 就可以**使用** System类的**静态方法和静态域**，而不**必加类名前缀**： 

`out.println("Goodbye, World!"); //i.e.,   System.out` 

### 将类放入包中

要想将一个类放人包中， 就必须将包的名字放在源文件的开头，包中定义类的代码之前。

`package ...`

如果没有在源文件中放置 package语句， 这个源文件中的类就被放置在一个默认包 ( default package) 中。默认包是一个没有名字的包。

将包中的文件放到与完整的包名匹配的子目录中。例如，com.horstmann.corejava 包 中的所有源文件应该被放置在子目录 com/horstmann/corejava中。编译器将类文件也放在相同的目录结构中。

##　类路径

 类文件也可以存储在JAR(Java归档）文件中。在一个 JAR 文件中， 可以包含 多个压缩形式的类文件和子目录， 这样既可以节省又可以改善性能。

JAR 文件使用 ZIP 格式组织文件和子目录。可以使用所有 ZIP 实用程序查看内部 的 rt.jar 以及其他的 JAR 文件。 

为了使类能够被多个程序共享，需要做到下面几点： 

1 ) 把类放到一个目录中， 例如 /home/user/classdir。需要注意， 这个目录是包树状结构 的基目录。如果希望将 com.horstmann.corejava.Employee类添加到其中，这个 Employee.class 类文件就必须位于子目录 /home/user/classdir/com/horstmann/corejava中。

2 ) 将 JAR 文件放在一个目录中，例如：/home/user/archives。 

3) 设置类路径（class　path)。类路径是所有包含类文件的路径的集合。 

在UNIX 环境中， 类路径中的不同项目之间采用冒号（:）分隔： `/home/user/classdir:.:/home/user/archives/archive.jar`

而在 Windows 环境中，则以分号（;）分隔： 

`c:\classdir;.;c:\archives\archive.jar`

类路径包括： 

•基目录/home/user/classdir(类UNIX环境) 或 c:\classdir(Windows环境)； 

•当前目录 (.)； 

•JAR 文件 /home/user/archives/archive.jar或c:\archives\archive.jar

由于运行时库文件（rt.jar 和在jre/lib 与jre/lib/ext 目录下的一些其他的 JAR 文件）会被 自动地搜索， 所以不必将它们显式地列在类路径中。 

类路径所列出的目录和归档文件是搜寻类的起始点。下面看一个类路径示例：

`/home/user/classdir:.:/home/user/archives/archive.jar` 

假定虚拟机要搜寻 com.horstmann.corejava.Employee 类文件。它首先要查看存储在jre/ lib 和jre/lib/ext 目录下的归档文件中所存放的系统类文件。显然，在那里找不到相应的类文 件，然后再查看类路径。然后查找以下文件： 

` /home/user/classdir/com/horstmann/corejava/Employee.class`

`com/horstmann/corejava/Employee.class` 从当前目录开始

`com/horstmann/corejava/Employee.class inside /home/user/archives/archive.jar `

仅可以导人其他包中的**公有类**。当然，也可以从**当前包**中导入非公有类。（非共有类默认访问权限为friend，当前包可以访问）

---

**综上所述，class path 拼接上 import 的路径就代表要寻找的类的绝对路径。**

---

## 文档注释

JDK 包含一个很有用的工具，叫做javadoc, 它可以由源文件生成一个 HTML 文档。

如果在源代码中添加以专用的定界符 /**开始的注释， 那么可以很容易地生成一个看上 去具有专业水准的文档。这是一种很好的方式，因为这种方式可以将代码与注释保存在一个地方。在修改源代码的同时， 重新运行javadoc 就可以轻而易举地保持两者的一致性。

### 注释的插入

每个 /** . . . */ 文档注释在标记之后紧跟着自由格式文本（free-form text)。标记由@开 始， 如@author 或@param。 

自由格式文本的第一句应该是一个概要性的句子。javadoc 实用程序自动地将这些句子抽 取出来形成概要页。 

在自由格式文本中，可以使用 HTML 修饰符， 例如，用于强调的 \<em>...\</em>、 用于 着重强调的 \<strong>...\</strong> 以及包含图像的 \<img ..> 等。不过，一定不要使用 \<hl> 或 \<hr>, 因为它们会与文档的格式产生冲突。

若要键入等宽代码，需要使用{@code...}而不是\<code>...\<code> ，因为这样不同担心代码中的’<‘字符转义。（比如代码中有<或者>就要进行转义，防止javadoc识别成html标签，而{@code}就没有这个问题）

### 类注释

类注释必须放在 import 语句之后，类定义之前。

---

生成的文档默认不显示出来比如 @author XXX 。如果显示的话需要额外在添加参数

---

例子：

```java
/**
 * 这是注释 
 * <em>作者</em>
 * @author XXXX ->//这个默认不显示
 */
public class Test{
    
}
```

### 方法注释

每一个方法注释必须放在所描述的方法之前。除了通用标记之外， 还可以使用下面的标记： 

- @param 变量描述  这个标记将对当前方法的“ param” （参数）部分添加一个条目。这个描述可以占据多 行， 并可以使用 HTML 标记。一个方法的所有@param 标记必须放在一起。 
- @return 描述 这个标记将对当前方法添加“ return” （返回）部分。这个描述可以跨越多行， 并可以 使用 HTML 标记。 
- @throws 类描述 这个标记将添加一个注释， 用于表示这个方法有可能抛出异常。

### 域注释

只需要对**公有域**（通常指的是静态常量）建立文档。

```java
/**
* 这是一个静态变量
*/
public static int flag = 1;
```

### 通用注释

下面的注释有一些不显示在javadoc生成的文档中。

可以用在类文档的注释：

- @author 姓名 产生一个（作者）条目
- @version 文本 产生一个（版本）条目

可以用在所有文档中的注释中：

- @since 文本 产生一个（始于）条目。这里的文本可以是引入特性的版本的描述 @since version 1.7.1
- @deprecated 文本 对类或方法，变量添加一个不在使用的注释，提出取代建议。如：@deprecated Use {@code setVisible(true)} instead 通过@see 和 @link 链接到文档相关部分或外部。
- @see 增加一个超链接，可以用于类也可以用于方法中。引用可选择：

package.class#feature label

\<a herf="">label\</a>

“text”

第一种情况是最常见的。只要提供类、方法或变量的名字，javadoc 就在文档中插入 一个超链接。例如， 

`@see com.horstraann.corejava.Employee#raiseSalary(double)` 建立一个链接到 com.horstmann.corejava.Employee 类的 raiseSalary(double) 方法的超 链接。 可以省略包名， 甚至把包名和类名都省去，此时，链接将定位于当前包或当前类。

需要注意，一定要使用井号（#)，而不要使用句号（.）分隔类名与方法名，或类 名与变量名。Java 编译器本身可以熟练地断定句点在分隔包、 子包、类、内部类与方 法和变量时的不同含义。但是javadoc 实用程序就没有这么聪明了，因此必须对它提 供帮助。 

如果@see 标记后面有一个双引号（"）字符，文本就会显示在 “ 另请参阅” 部分。 

### 包与概述注释 

可以直接将类、方法和变量的注释放置在 Java 源文件中，只要用 /** . . . */ 文档注释界 定就可以了。但是， 要想产生**包注释**，就需要在每一个包目录中添加一个单独的文件。可以 有如下两个选择：

1 ) 提供一个以 package.html 命名的 HTML 文件。在标记 \<body>—\</body> 之间的所有 文本都会被抽取出来。

2 )  提供一个以 package-info.java命名的 Java 文件。这个文件必须包含一个初始的以 /** 和 */ 界定的 Javadoc 注释， 跟随在一个包语句之后。它不应该包含更多的代码或注释。 

还可以为所有的源文件提供一个概述性的注释。这个注释将被放置在一个名为 overview, html 的文件中，这个文件位于包含所有源文件的父目录中。标记 \<body>... \</body> 2间的所 有文本将被抽取出来。当用户从导航栏中选择“ Overview” 时，就会显示出这些注释内容。 

#### 注释的抽取

这里，假设 HTML 文件将被存放在目录 docDirectory 下。执行以下步骤： 

1 ) 切换到包含想要生成文档的源文件目录。如果有嵌套的包要生成文档， 例如 com. horstmann.corejava, 就必须切换到包含子目录 com 的目录（如果存在 overview.html 文件的 话， 这也是它的所在目录)。

2 ) 如果是一个包，应该运行命令: 

`javadoc -d docDirectory nameOfPackage`

或对于多个包生成文档，运行:

`javadoc -d docDirectory nameOfPackage1 nameOfPackage2...`

如果文件在默认包中，运行：

`javadoc -d docDirectory *.java`

如果省略了 -d docDirectory 选项， 那 HTML 文件就会被提取到当前目录下。这样有可能 会带来混乱，因此不提倡这种做法。 

可以使用多种形式的命令行选项对javadoc 程序进行调整。例如， 可以使用-author 和 -version选项在文档中包含@author 和@version标记（**默认情况下，这些标记会被省 略**)。另一个很有用的选项是-link, 用来为标准类添加超链接。例如， 如果使用命令 

`javadoc -link http://docs.oracle.eom/:javase/8/docs/api *.java `

那么，所有的标准类库类都会自动地链接到 Oracle 网站的文档。



 

