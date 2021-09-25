---
title: Java 继承
tags:
- Java
- Java核心技术卷1读书笔记
categories: 编程
---

## 类、超类和子类

### 定义子类

在java中所有继承都是共有继承。

```java
public class Manager extends Employee｛
｝
```

关键字 extends 表明正在构造的新类派生于一个已存在的类。 

 已存在的类称为超类 ( superclass)、 基类（base class) 或父类（parent class); 

新类称为子类（subclass)、 派生类 (derived class) 或孩子（child class)。

前缀“ 超” 和“ 子” 来源于计算机科学和数学理论中的集合语言的术语。所有雇 员组成的集合包含所有经理组成的集合。可以这样说， 雇员集合是经理集合的超集， 也 可以说，经理集合是雇员集合的子集。 

### 覆盖方法

超类Employee的有一个私有域salary，子类Manager有一个私有域bonus，超类和子类都有一个同名方法getSalary()。Employee的薪水只有薪水，Manager的薪水是薪水+奖金。那么子类Manager的薪水为：

```java
public double getSalary(){
    return salary+bouns;//错误
}
```

因为 Manager 类的 getSalary() 方法不能够直接地访问超类的私有域。

只有 Employee 类的方法才能够访问私有部 分。如果 Manager 类的方法一定要访问私有域， 就必须借助于公有的接口， Employee类中的 公有方法 getSalary 正是这样一个接口。

 现在，再试一下。将对 salary 域的访问替换成调用超类的getSalary方法：

```java
public double getSalary(){
    return super.getSalaty()+bouns;//正确
}
```

有些人认为 super 与 this 引用是类似的概念， 实际上，这样比较并不太恰当。这是 因为 super 不是一个对象的引用， 不能将 super 赋给另一个对象变量， 它只是一个指示编 译器调用超类方法的特殊关键字。

在子类中可以增加域、 增加方法或覆盖超类的方法，然而绝对 不能删除继承的任何域和方法。 

### 子类构造器

给Manager提供一个构造器:

```java
public Manager(String name, double salary, int year, int month, int day) {
    super(name, salary, year, month, day); 
    bonus = 0; 
}
```

这里的关键字 super 具有不同的含义。语句 

`super(n, s, year, month, day); `

是“ 调用超类 Employee 中含有 n、s、year month 和 day 参数的构造器” 的简写形式。 

由于 Manager 类的构造器不能访问 Employee 类的私有域， 所以必须利用 Employee 类 的构造器对这部分私有域进行初始化，我们可以通过 super 实现对超类构造器的调用。**使用 super 调用构造器的语句必须是子类构造器的第一条语句**。 

如果子类的构造器没有显式地调用超类的构造器， 则将自动地调用超类默认（没有参数) 的构造器。 如果超类没有不带参数的构造器， 并且在子类的构造器中又没有显式地调用超类 的其他构造器 ，则 Java 编译器将报告错误。 

------

 回忆一下， 关键字 this 有两个用途： 一是引用隐式参数，二是调用该类其他的构造器 ， 同样，super 关键字也有两个用途：一是调用超类的方法，二是调用超类的构造器。 在调用构造器的时候， 这两个关键字的使用方式很相似。调用构造器的语句只能作为另一个构造器的第一条语句出现。构造参数既可以传递给本类（this) 的其他构造器，也可以传递给超类（super) 的构造器。 

---

```java
Manager boss = new Manager("Carl Cracker", 80000，1987, 12, 15);
Emplyee e = boss;//父类指向子类对象
e.getSalary();//调用子类的方法
```

当e引用 Employee 对象时，e.getSalary( ) 调用的是 Employee 类中的 getSalary 方法；当 e 引用 Manager 对象时，e.getSalary( ) 调用的是 Manager类中的 getSalary 方法。虚拟机知道 e 实际引用的对象类型，因此能够正确地调用相应的方法。

 一个对象变量（例如， 变量 e) 可以指示多种实际类型的现象被称为**多态**（polymorphism)。 在运行时能够自动地选择调用哪个方法的现象称为动态绑定（dynamic binding)。

### 多态

有一个用来判断是否应该设计为继承关系的简单规则， 这就是“ is-a” 规则，它表明子类的每个对象也是超类的对象。例如，每个经理都是雇员。 因此，将 Manager 类设计为 Employee 类的子类是显而易见的，反之不然，并不是每一名雇员都是经理。

“is-a” 规则的另一种表述法是置换法则。它表明程序中**出现超类对象的任何地方都可以 用子类对象置换**。 

```java
Employee e; 
e = new Employee(. . .); // Employee object expected 
e =new Manager(. . .); // OK, Manager can be used as well 
```

在 Java程序设计语言中，对象变量是多态的。 一个 Employee 变量既可以引用一个 Employee 类对象， 也可以引用一个 Employee 类的任何一个子类的对象。

然而，不能将一个超类的引用赋给子类变量。例如，下面的赋值是非法的 

`Manager m = new Employee(); // Error`

原因很清楚：不是所有的雇员都是经理。 如果赋值成功，m 有可能引用了一个不是经理的 Employee 对象， 当在后面调用 m.setBonus(...) 时就有可能发生运行时错误。 

### 理解方法调用

弄清楚如何在对象上应用方法调用非常重要。下面假设要调用 x.f(args)，隐式参数 x声 明为类 C 的一个对象。下面是调用过程的详细描述： 

1 ) 编译器査看对象的**声明类型和方法名**。假设调用 x.f(param)，且隐式参数 x声明为 C 类的对象。需要注意的是有可能存在多个名字为 f, 但参数类型不一样的方法。。编译器将会一一列举所有 C 类中名为 f 的方法和其超类中 访问属性为 public 且名为 f 的方法（超类的私有方法不可访问） 。 

至此， 编译器已获得所有可能被调用的候选方法。 

2 ) 接下来，编译器将査看调用方法时**提供的参数类型**。如果在所有名为 f 的方法中存在 一个与提供的参数类型完全匹配，就选择这个方法。这个过程被称为重载解析（overloading resolution)。

由于允许类型转换（int 可以转换成double, Manager 可以转换成 Employee, 等等)， 所以这 个过程可能很复杂。如果编译器没有找到与参数类型匹配的方法， 或者发现经过类型转换后 有多个方法与之匹配， 就会报告一个错误。 

至此， 编译器已获得需要调用的方法名字和参数类型。

---

前面曾经说过，方法的名字和参数列表称为**方法的签名**。例如，f(int) 和 f(String) 是两个具有相同名字， 不同签名的方法。如果在子类中定义了一个与超类签名相同的方法， 那么子类中的这个方法就覆盖了超类中的这个相同签名的方法。

不过，返回类型不是签名的一部分， 因此，在覆盖方法时， 一定要保证返回类型 的兼容性。 **允许子类将覆盖方法的返回类型定义为原返回类型的子类型或者相同。**

---

3 ) 如果是 private 方法、 static 方法、final 方法或者构造器， 那么编译器将可以准确地知道应该调用哪个方法， 我们将这种调用方式称为**静态绑定（static binding)**。与此对应的是，调用的方法依赖于隐式参数的实际类型， 并且在运行时实现动态绑定。在我们列举的示例中， 编译器采用动态绑定的方式生成一条调用 f (String) 的指令。 

4 ) 当程序运行，并且采用**动态绑定**调用方法时， 虚拟机一定调用与 x 所引用对象的实际类型最合适的那个类的方法。**假设 x 的实际类型是 D，它是C类的子类。如果 D类定义了 方法 f(String)，就直接调用它；否则，将在 D类的超类中寻找 f(String)， 以此类推。** 

每次调用方法都要进行搜索，时间开销相当大。因此，虚拟机预先为每个类创建了一个 **方法表**（method table), 其中列出了所有方法的签名和实际调用的方法。这样一来，在真正 调用方法的时候， 虚拟机仅查找这个表就行了。

---

**在覆盖一个方法的时候，子类方法不能低于超类方法的可见性。特别是， 如果超类方法是 public, 子类方法一定要声明为 public。**经常会发生这类错误：在声明子类方法的时 候， 遗漏了 public 修饰符。此时，编译器将会把它解释为试图提供更严格的访问权限。 

---

### 阻止继承：final 类和方法 

有时候，可能希望阻止人们利用某个类定义子类。不允许扩展的类被称为 final 类。

```java
public final class Executive extends Manager{
    ...
}
```

类中的特定方法也可以被声明为 final。如果这样做，子类就不能覆盖这个方法（final 类中的所有方法自动成为final 方法，但不包括域，final域是构造后不可改变值）。

### 强制类型转换

正像有时候需要将浮点型数值转换成整型数值一样，有时候也可能需要将某个类的对象 引用转换成另外一个类的对象引用。对象引用的转换语法与数值表达式的类型转换类似， 仅 需要用一对圆括号将目标类名括起来，并放置在需要转换的对象引用之前就可以了。

```java
Employee e = new Manager();//子类对象声明为父类类型后，可强制转型
Manager boss = (Manager) e;//下转型
```

进行类型转换的唯一原因是：在暂时忽视对象的实际类型之后，使用对象的全部功能。 比如我们让boss访问Manager新增的变量和方法。(**e 是不能访问 Manager 新增的方法和变量的**)

将一个值存入变量时， 编译器将检查是否允许该操作。将一个子类的引用赋给一个超类变量，编译器是允许的。但将一个超类的引用赋给一个子类变量， 必须进行类型转换， 这样 才能够通过运行时的检査。 

如果试图在继承链上进行向下的类型转换，并且“ 谎报” 有关对象包含的内容， Java运行时系统将报告这个错误，并产生一个 ClassCastException 异常。 

```java
Emplyee e = new Employee();//根本没有子类的事情
Manager boss = (Manager) e;//错误
```

应该养成这样一个良好的程序设计习惯： 在进行类型转换之前，先查看一下是否能够成功地转换。这个过程简单地使用 instanceof操作符(A instanceof B A是B的实例或子类？)就可以实现。 例如： 

```java
if(e instanceof Manager){
    boss = (Manager) e;
}
```

实际上，通过类型转换调整对象的类型并不是一种好的做法。在我们列举的示例中， 大 多数情况并不需要将 Employee 对象转换成 Manager 对象， 两个类的对象都能够正确地调用 getSalary方法，这是因为实现多态性的动态绑定机制能够自动地找到相应的方法。 

只有在使用 Manager 中特有的方法时才需要进行类型转换， 例如， setBonus 方法 如果 鉴于某种原因，发现需要通过 Employee 对象调用 setBonus方法， 那么就应该检查一下超类 的设计是否合理。重新设计一下超类，并添加 setBonus法才是正确的选择。请记住，只要 没有捕获 ClassCastException异常，程序就会终止执行。 在一般情况下，应该尽量少用类型转换和 instanceof 运算符。

### 多态与继承及instanceof的一个例子

```java
class Animal{//父类
	public void eat() {
		System.out.println("animal eat");
	}
	public void eatAnimal() {
		System.out.println("animal eatAnimal");
	}
}

class Cat extends Animal{//子类
	public void eat() {
		System.out.println("cat eat");
	}
	public void eatFish() {
		System.out.println("cat eatFish");
	}
}
public class DT {
public static void main(String[] args) {
	Ainmal a = new Cat();
	Cat b = new Cat();
	a.eat();//cat eat Cat子类重写了eat方法，调用子类的方法,是多态
	((Cat)a).eatFish();//cat eatFish 直接a.eatFish会报错，需要强制类型转换，Animal类没有eatFish方法
	a.eatAnimal();//animal eatAnimal Animal类里的eatAnimal方法被调用
	b.eatFish();//cat Fish Cat类的eatFish方法被调用
	b.eatAnimal();//animal eatAnimal Cat类继承的eatAnimal方法被调用
    
    //instanceof
    Animal c = new Animal();  
    System.out.println(a instanceof Animal);//true
	System.out.println(a instanceof Cat);//true
	System.out.println(b instanceof Animal);//true
	System.out.println(c instanceof Cat); //false
}
}
```

### 抽象类

使用 abstract 关键字，这样就完全不需要实现这个方法。

```java
public abstract String getDescription(){}
```

**为了提高程序的清晰度， 包含一个或多个抽象方法的类本身必须被声明为抽象的。** 

```java
public abstract class Test{}
```

抽象类可以包含普通方法。

抽象方法充当着占位的角色， 它们的具体实现在子类中。扩展抽象类可以有两种选择。 一种是在抽象类中定义部分抽象类方法或不定义抽象类方法，这样就必须将子类也标记为抽 象类；另一种是定义全部的抽象方法，这样一来，子类就不是抽象的了。 

类即使不含抽象方法，也可以将类声明为抽象类。 

抽象类不能被实例化。

### 受保护访问

然而，在有些时候，人们希望超类中的某些方法允许被子类访问， 或允许子类的方法访 问超类的某个域。为此， 需要将这些方法或域声明为 protected。

下面归纳一下 Java 用于控制可见性的 4 个访问修饰符： 

1 ) 仅对本类可见 private。

2 ) 对所有类可见 public。

3 ) 对本包和所有子类可见 protected。 

4 ) 对本包可见 默认， 不需要修饰符。

## Object： 所有类的超类

Object 类是 Java 中**所有类**的始祖。 在 Java 中每个类都是由它扩展而来的。但是并不需 要这样写： 

```java
public class Test extends Object{}
```

如果没有明确地指出超类，Object 就被认为是这个类的超类。

在 Java 中，只有基本类型（primitive types) 不是对象， 例如，数值、 字符和布尔类型的 值都不是对象。 

所有的数组类塱，不管是对象数组还是基本类型的数组都扩展了 Object 类。 

### equals 方法

Object 类中的 equals方法用于检测一个对象是否等于另外一个对象。在 Object 类中，这个方法将判断两个对象**是否具有相同的引用**。如果两个对象具有相同的引用， 它们一定是相等的。不过我们一般比较值是否相同，所以一般需要重写该方法。

下面给出编写一个完美的 equals方法的建议： 

1 ) 显式参数命名为 otherObject, 稍后需要将它转换成另一个叫做 other 的变量。

 2 ) 检测 this 与 otherObject 是否引用同一个对象： 

if (this = otherObject) return true;

 这条语句只是一个优化。实际上，这是一种经常采用的形式。因为计算这个等式要比一 个一个地比较类中的域所付出的代价小得多。 

3 ) 检测 otherObject 是否为 null, 如果为 null, 返 回 false。这项检测是很必要的。 

if (otherObject = null) return false; 

4 ) 比较 this 与 otherObject 是否属于同一个类。如果 equals 的语义在每个子类中有所改变（父类与子类比较），就使用 getClass 检测： 

if (getClass() != otherObject.getCIassO) return false;

 如果所有的子类都拥有统一的语义（对象统一），就使用 instanceof检测： 

if (!(otherObject instanceof ClassName)) return false; 

5 ) 将 otherObject 转换为相应的类类型变量： 

ClassName other = (ClassName) otherObject 

6 ) 现在开始对所有需要比较的域进行比较了。使用 == 比较基本类型域，使用 equals 比 较对象域。如果所有的域都匹配， 就返回 true; 否则返 回 false。 

如果在子类中重新定义 equals, 就要在其中包含调用super.equals(other)。 

### hashCode 方法

散列码（ hash code) 是由对象导出的一个整型值。散列码是没有规律的。如果 x 和 y 是 两个不同的对象， x.hashCode( ) 与 y.hashCode( ) **基本上**不会相同。 

如果重新定义 equals方法，就必须重新定义 hashCode方法， 以便用户可以将对象插人到散列表中（第一行比较this==otherObject）。

需要组合多个散列值时，可以调用 ObjeCtS.hash 并提供多个参数。这 个方法会对各个参数调用 Objects.hashCode， 并组合这些散列值。

```java
public int hashCode(){
    return Object.hash(name,salary,hireDay);
}
```

Equals 与 hashCode 的定义必须一致：如果 x.equals(y) 返回 true, 那么 x.hashCode( ) 就必 须与 y.hashCode( ) 具有相同的值。

### toString 方法

在 Object 中还有一个重要的方法， 就是 toString方法， 它用于返回表示对象值的字符 串。

绝大多数（但不是全部）的 toString方法都遵循这样的格式：类的名字，随后是一对方括号括起来的域值。

```java
public String toString(){
    return getClass().getName()+"[name="+name+",salary"=salary+"]";
}
```

最好通过调用getClass( ).getName( ) 获得类名的字符串,而不要将类名硬加到toString()方法中。

Object 类定义了 toString方法， 用来打印输出对象所属的类名和散列码。

## 泛型数组列表

ArrayList 是一个采用类型参数（type parameter) 的泛型类（generic class)。为了指定数 组列表保存的元素对象类型，需要用一对尖括号将类名括起来加在后面， 例如，ArrayList \<Employee>。

下面声明和构造一个保存 Employee 对象的数组列表： `ArrayList<Employee> staff = new ArrayList<Employee>; `

两边都使用类型参数 Employee， 这有些繁琐。Java SE 7中， 可以省去右边的类型参数： 

`ArrayList<Employee> staff = new ArrayList<>；`

使用add 方法可以将元素添加到数组列表中。

如果已经清楚或能够估计出数组可能存储的元素数量， 就可以在填充数组之前调用 ensureCapacity方法： 

`staff.ensureCapacity(lOO); `

另外，还可以把初始容量传递给 ArrayList 构造器： 

`ArrayList<Employee> staff = new ArrayList(100);`

size方法将返回数组列表中包含的实际元素数目。

一旦能够确认数组列表的大小不再发生变化，就可以调用 trimToSize方法。这个方法将 存储区域的大小调整为当前元素数量所需要的存储空间数目。垃圾回收器将回收多余的存储 空间。

一旦整理了数组列表的大小，添加新元素就需要花时间再次移动存储块，所以应该在确 认不会添加任何元素时， 再调用 trimToSize。

### 访问数组列表元素

 使用 get 和 set 方法实现访问或改变数组元素的操作，而不使用人们喜爱的 [ ]语法格式。 

只有 i 小于或等于数组列表的大小时， 才能够调用 list.set(i, x)。

使用 add 方法为数组添加新元素， 而不要使用 set 方法， 它只能替换数组中已经存在 的元素内容。 

使用下列格式获得数组列表的元素: 

`Employee e = staff.get(i);`

## 对象包装器与自动装箱

有时， 需要将 int 这样的基本类型转换为对象。所有的基本类型都冇一个与之对应的类。 例如Integer 类对应基本类型 int。通常， 这些类称为包装器 （ wrapper) 这些对象包装器类 拥有很明显的名字：Integer、Long、Float、Double、Short、Byte、Character、Void 和 Boolean (前 6 个类派生于公共的超类 Number)。对象包装器类是不可变的，即一旦构造了包装器，就不 允许更改包装在其中的值。同时， 对象包装器类还是 final, 因此不能定义它们的子类。 

假设想定义一个整型数组列表。而尖括号中的类型参数不允许是基本类型，也就是说， 不允许写成 ArrayList\<int>。这里就用到了 Integer 对象包装器类。我们可以声明一个 Integer 对象的数组列表。 

`ArrayList<Integer> list = new ArrayList<Integer>()`

由于每个值分别包装在对象中， 所以 ArrayList\<lnteger> 的效率远远低于 int[ ] 数组。 因此， 应该用它构造小型集合，其原因是此时程序员操作的方便性要比执行效率更加重要。

 幸运的是， 有一个很有用的特性，从而更加便于添加 int类型的元素到 ArrayLisKlntegeP 中。下面这个调用 

`list.add(3)`

将会自动转变为：

`list.add(Integer.Valueof(3))`

这种变换被称为**自动装箱**（autoboxing)。

相反地， 当将一个 Integer 对象赋给一个 int 值时， 将会自动地拆箱。也就是说， 编译器将下列语句：

`int n = list. get(i);`

翻译成

`int n = list.get(i).intValue();`

大多数情况下，容易有一种假象， 即基本类型与它们的对象包装器是一样的，只是它们 的相等性不同。大家知道， == 运算符也可以应用于对象包装器对象， 只不过检测的是对象是 否指向同一个存储区域， 因此，下面的比较通常不会成立： 

```java
Integer a = 1000; 
Integer b = 1000; 
if (a = b) ... 
```

然而，Java 实现却有可能（may) 让它成立。如果将经常出现的值包装到同一个对象中， 这种比较就有可能成立。这种不确定的结果并不是我们所希望的。解决这个问题的办法是在 两个包装器对象比较时调用 equals方法。 

自动装箱规范要求 boolean、byte、char<=127， 介于 -128 ~ 127 之间的 short 和 int 被包装到固定的对象中。例如，如果在前面的例子中将 a 和 b 初始化为 100，对它们进行比较的结果一定成立。 

---

有些人认为包装器类可以用来实现修改数值参数的方法， 然而这是错误的。由于 Java 方法都是值传递， 所以不可能编写一个下面这样的能够增加 整型参数值的 Java 方法: 

```java
public static void triple(int x) // won't work 
{ x = 3 * x; // modifies local variable 
}
```

将 int 替换成 Integer :

```java
public static void triple(Integer x) // won't work
{ x = 3 *x;
}
```

问题是 Integer 对象是不可变的： 包含在包装器中的内容不会改变: 不能使用这些包 装器类创建修改数值参数的方法。 

如果想编写一个修改数值参数值的方法， 就需要使用在 org.omg.CORBA 包中定义的 **持有者（ holder) 类型**， 包括 IntHolder、BooleanHolder 等。每个持有者类型都包含'一个 公有（！）域值，通过它可以访问存储在其中的值。 

```java
public static void triple(IntHolder x) 
{ x.value = 3 * x.value; 
} 
```

## 参数数量可变的方法

在 Java SE 5.0 以前的版本中，每个 Java方法都有固定数量的参数。然而，现在的版本 提供了可以用可变的参数数量调用的方（有时称为“ 变参” 方法)。 

```java
public static double max(double... values) { 
    double largest = Double.NECATIVEJNFINITY; 
    for (double v : values) 
        if (v > largest) 
            largest = v; return largest; 
}
```

这里的省略号 . . . 是 Java 代码的一部分，它表明这个方法可以接收任意数量的double 类型，存放在values[]数组中。

可以像下面这样调用这个方法： 

double m = max(3.1 , 40.4, -5); 

编译器将 new double[ ] {3.1, 40.4,-5} 传递给 max方法。 

## 枚举类

在定义枚举类型时我们使用的关键字是enum，与class关键字类似，：

`public enum Size { SMALL, MEDIUM, LARGE, EXTRAJARGE }; `

实际上， 这个声明定义的类型是一个类， 它刚好有 4 个实例， 在此尽量不要构造新对象。 

因此， 在比较两个枚举类型的值时， 永远不需要调用 equals, 而直接使用“ = =” 就 可以了。 

定义一个枚举类：

```java
enum Size{
	SMAll("s",1),MEDIUM("M",2);//会调用第5行的构造函数，补充信息
	private String desc;
	private int id;
	private Size(String desc,int id) {//构造函数
		this.desc=desc;
		this.id = id;
	}
	public String getDesc() {//通过Size.SMAll.getDesc()调用，获得信息
		return desc+" "+id;
	}
}
```

所有的枚举类型都是 Enum 类的子类。它们继承了这个类的许多方法。其中最有用的一 个是 toString， 这个方法能够返回枚举常量名。例如， Size.SMALL.toString( ) 将返回字符串 “ SMALL”。 

## 反射

能够分析类能力的程序称为反射（reflective)。 反射机制可以用来： 

•在运行时分析类的能力。 

•在运行时查看对象， 例如， 编写一个 toString方法供所有类使用。

 •实现通用的数组操作代码。 

•利用 Method 对象， 这个对象很像中的函数指针。

### Class 类

在程序运行期间，Java运行时系统始终为所有的对象维护一个被称为运行时的类型标识。 这个信息跟踪着每个对象所属的类。虚拟机利用运行时类型信息选择相应的方法执行。 

---

在java语言中，万事万物皆对象，但是静态成员不是对象（静态成员属于类），普通数据类型不是对象。

**类也是对象，类是java.lang.Class 类的实例对象。** 这个对象称为类类型（class type）

---

### 三种实例化Class的实例对象的方法

1、可以通过专门的 Java 类访问这些信息。保存这些信息的类被称为 Class, 这个名 字很容易让人混淆。Object 类中的 getClass( ) 方法将会返回一个 Class 类型的实例。 

 ```java
Manage m = new Manager();
Class c = m.getClass();
syso(c.getName());//输出 class 包名.Manager;
 ```

2、还可以调用静态方法 forName(String classname) 获得类名对应的 Class 对象。 classname 要带上包名。

```java
Class c1 = Class.forName(c.getName());
```

如果类名保存在字符串中，并可在运行中改变， 就可以使用这个方法。当然， 这个方法 只有在 classname 是类名或接口名时才能够执行。否则，forName 方法将抛出一个 checked exception (已检查异常） 。无论何时使用这个方法， 都应该提供一个异常处理器（exception handler)。

3、第三种方法时通过T.class 获得Class类型，例如：

```java
Class cl1 = Random.class;//引入Random包-》class 是隐形的静态成员变量
Class cl2 = int.class;
Class cl3 = Double[].class;
```

请注意，一个 Class 对象实际上**表示的是一个类型**，而这个类型未必一定是一种类。例如， int 不是类，但 int.class 是一个 Class 类型的对象。 

虚拟机为每个类型管理一个 Class 对象。因此， 可以利用==运算符实现两个类对象比较的操作。 

```java
if(m.getClass()==Manager.class)
```

**一个类只可能是Class 类的一个实例对象。**

### 通过类类型创建对象

有一个很有用的方法 newlnstance( )， 可以用来动态地创建一个类的实例：

```java
m.getClass0.newlnstance(); 
```

创建了一个与 e具有相同类类型的实例。newlnstance方法调用默认的构造器（没有参数的构造器）初始化新创建的对象。如果这个类没有默认的构造器， 就会抛出一个异常。

将 forName 与 newlnstance 配合起来使用， 可以根据存储在字符串中的类名创建一个对象 。

### 利用反射分析类的能力 

在java.lang.reflect 包中有三个类 Field、Method 和 Constructor 分别用于描述类的域、 方法和构造器。（域是Field类的对象，方法是Method的对象，构造器是Constructor的对象）。

例子，Method类获得类的方法。

```java
Class e = Manager.class;
Method m = e.getMethods();
syso(m[0]);//获得Manager类的一个方法名字
```

 想获得类的其它信息同理，查阅API手册。

