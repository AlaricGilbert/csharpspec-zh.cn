# 简介

C#(读作"See Sharp")是一门简约，现代化，面向对象且类型安全的编程语言。C#由C和C++衍生而出并对于C、C++与Java程序员很容易上手。C#作为 ***ECMA-334*** 与 ***ISO/IEC 23270*** 已分别被ECMA国际和国际标准化组织标准化。微软的针对 .NET Framework的C#编译器实现同时符合这些标准。

C#是一门面向对象的语言，但C#更多的为 ***面向组件*** 编程提供了支持。现代化的软件设计日益依赖于内建与自描述的功能包形式的组件。这些组件的关键是它们呈现具有属性，方法和事件的编程模型；它们具有提供有关组件的声明性信息的属性；并且它们嵌入了自己的文档。C#提供了直接支持这些概念的语言结构，使得C#成为一种可以非常自然地创建和使用软件组件的语言。

一些C#特性有助于构建坚固耐用的应用: ***垃圾回收*** 自动回收未使用对象占用的内存；***异常处理*** 提供了一种结构化和可扩展的错误检测和恢复方法而这门语言 ***类型安全*** 的设计使得读取未初始化的变量，索引超出其边界的数组或执行未经检查的类型转换是不可能的。

C#拥有 ***统一类型系统***。一切的C#类型，包括以`int`与`double`为代表的原始类型，均继承自单一的根类型`object`。因此，所有类型共享同一组基本操作，并且任何类型的值可以以一同一方式存储、传输或操作。此外，C#支持用户定义的引用类型和值类型，允许动态分配对象以及轻量级结构的内联存储。

为了确保C#程序和库能随着时间的推移以兼容的方式发展，在C#的设计中，人们越来越重视 ***版本化***。许多编程语言很少关注这个问题，故当引入新版本的依赖库时，使用这些语言编写的程序会比平时更频繁地中断。直接受版本化考虑因素影响的C#设计方面分别包括`virtual`和`override`修饰符，方法重载解析的规则以及对显式接口成员声明的支持。

本章的其余部分描述了C＃语言的基本功能。虽然后面的章节以面向细节的方式描述了规则和例外，有时甚至是数学方式，但本章的目的是为了清晰和简洁而牺牲完整性。目的是向读者提供有助于编写早期程序和阅读后续章节的语言介绍。

## Hello world

"Hello, World" 程序通常用于介绍一门编程语言。下面是在C#中的一个实现：

```csharp
using System;

class Hello
{
    static void Main() {
        Console.WriteLine("Hello, World");
    }
}
```

C#源文件的文件扩展名通常是`.cs`。假设“Hello，World”程序存储在文件`hello.cs`中，可以使用在命令行使用Microsoft C#编译器编译：
```
csc hello.cs
```
它生成一个名为`hello.exe`的可执行程序集。此应用程序运行时产生的输出是：
```
Hello, World
```

该"Hello，World"程序以一个引用`System`命名空间的`using`指令开始。命名空间提供了组织C#程序和库的分层的方法。命名空间可以包含类型和其他命名空间——例如，`System`命名空间包含许多类型，例如程序中引用的`Console`类，以及许多其他命名空间，例如`IO`和`Collections`。引用给定命名空间的`using`指令允许非限定地使用作为该命名空间成员的类型。由于`using`指令，程序可以使用`Console.WriteLine`作为`System.Console.WriteLine`的简写。

被该"Hello, World"程序声明的类`Hello`有且只有一个成员：名称为`Main`的方法。该`Main`方法被`static`修饰符所修饰，表明其为一个静态方法。虽然实例方法可以使用关键字`this`引用特定的封闭对象的实例，但静态方法可以在不引用特定对象的情况下运行。按照惯例，程序的入口点常用名为`Main`的静态方法。

程序的输出由命名空间`System`中`Console`类的`WriteLine`方法产生。此类由 .NET Framework类库提供，默认情况下，它由Microsoft C#编译器自动引用。请注意，C#本身没有单独的运行时库。而 .NET Framework是C#的运行时库。

## 程序结构

***程序***，***名称空间***，***类型***， ***成员*** 和 ***程序集*** 是C#的关键组织概念。一个C#程序由一个或多个源文件组成。程序声明包含成员的类型并可以组织成命名空间。类和接口是类型的示例。字段，方法，属性和事件是成员的示例。编译C#程序时，它们将物理打包到程序集中。程序集通常具有文件扩展名“.exe”或“.dll”，具体取决于它们是实现 ***应用程序*** 还是 ***库***。

下面提供的例子：

```csharp
using System;

namespace Acme.Collections
{
    public class Stack
    {
        Entry top;

        public void Push(object data) {
            top = new Entry(top, data);
        }

        public object Pop() {
            if (top == null) throw new InvalidOperationException();
            object result = top.data;
            top = top.next;
            return result;
        }

        class Entry
        {
            public Entry next;
            public object data;
    
            public Entry(Entry next, object data) {
                this.next = next;
                this.data = data;
            }
        }
    }
}
```
在一个名为`Acme.Collections`的命名空间中声明了一个名为`Stack`的类。该类的完全限定名称是`Acme.Collections.Stack`。这个类含有一些成员：名为`top`的字段、两个分别名为`Push`和`Pop`的方法以及一个名为`Entry`的嵌套类。类`Entry`又包含三个成员：分别名为`next`与`data`的字段和一个 ***构造函数*** 。如果这个例子的源代码存储在文件`acme.cs`中，以下的命令行指令：

```
csc /t:library acme.cs
```
可以将这个例子作为一个类库（没有`Main`入口点的代码）编译并产生一个名为`acme.dll`的程序集。

这些程序集包含有 ***中间语言*** (Intermediate Language, IL) 指令形式的可执行代码，以及 ***元数据*** 形式的符号信息 (metadata)。在执行之前，程序集中的IL代码将由.NET公共语言运行时的即时编译器(JIT)自动转换为针对特定处理器的机器代码。

因为程序集是包含代码和元数据的自描述功能单元，所以C#中不需要`#include`指令和头文件。特定程序集中包含的公共类型和成员只需在编译程序时引用该程序集即可在C#程序中使用。例如，以下程序使用`acme.dll`程序集中的`Acme.Collections.Stack`类：

```csharp
using System;
using Acme.Collections;

class Test
{
    static void Main() {
        Stack s = new Stack();
        s.Push(1);
        s.Push(10);
        s.Push(100);
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
        Console.WriteLine(s.Pop());
    }
}
```
如果该源码存储在`test.cs`文件中，当编译`test.cs`时，可以使用编译器的`/r`选项引用`acme.dll`程序集：

```
csc /r:acme.dll test.cs
```
这将产生一个名为`test.exe`的可执行程序集，当我们运行它时将产生如下输出：

```
100
10
1
```
C#允许将程序的源文本存储在多个源文件中。编译多文件C#程序时，所有源文件将一起处理，源文件可以自由地相互引用——概念上，就好像所有源文件在处理之前被连接成一个大文件一样。C#中从不需要前向声明，因为除极少数例外情况外，声明顺序无关紧要。C#不限制源文件仅声明一个公共类型，也不要求源文件的名称与源文件中声明的类型匹配。

## 类型与变量

C#中有两种类型： ***值类型*** 与 ***引用类型***。值类型的变量直接包含它们的数据，而引用类型的变量存储对其数据的引用，后者称为对象。对于引用类型，两个变量可以引用同一个对象，因此对一个变量的操作可能会影响另一个变量引用的对象。对于值类型，变量每个都有自己的数据副本，并且一个上的操作不可能影响另一个(除了`ref`和`out`参数变量)。

C#的值类型分为 ***简单(原始)类型***、 ***枚举类型***、 ***结构类型*** 和 ***可空类型***，而C#的引用类型又分为 ***类***、***接口***、***数组*** 和 ***委托***.

下表概述了C#的类型系统。

| __类别__ |              |                  __描述__                    |
|---------|--------------|---------------------------------------------|
|  值类型  | 简单(原始)类型 |  有符号整数： `sbyte`, `short`, `int`, `long`  |
|         |              | 无符号整数： `byte`, `ushort`, `uint`, `ulong` |
|         |              |            Unicode字符： `char`               |
|         |              |       IEEE浮点数： `float`, `double`           |
|         |              |           高精度十进制小数: `decimal`           |
|         |              |               布尔型: `bool`                  |
|         |    枚举类型    |         用户定义的形式： `enum E {...}`        |
|         |    结构类型    |        用户定义的形式： `struct S {...}`       |
|         |    可空类型    |         具有`null`值的所有其他值类型的扩展       |
| 引用类型 |       类      |          所有其他类型的终极基类： `object`       |
|         |              |           Unicode字符串： `string`             |
|         |              |           用户定义的形式： `class C {...}`      |
|         |      接口     |         用户定义的形式： `interface I {...}`    |
|         |      数组     |     一维与多维数组，例如：`int[]` 与 `int[,]`     |
|         |      委托     |     用户定义的形式： `delegate int  D(...)`     |

八种整型提供了对有符号或无符号形式的8位、16位、32位及64位数值的支持。

两种浮点类型，`float` 与 `double`，分别代表使用32位单精度与64位双精度IEEE 754格式。

`decimal`类型是一种适合财务和货币计算的128位数据类型。

C#的`bool`类型用于表示布尔型的要么是`true` 要么是 `false`的值。

C#的字符与字符串处理使用Unicode编码。`char`类型代表一个UTF-16代码单元，而`string`类型代表一系列一系列UTF-16代码单元。

下表总结了C＃的数字类型：


|  __类别__ | __位数__ | __类型名__  | __值域/精度__ |
|----------|----------|-----------|---------------------|
| 有符号整型 | 8        | `sbyte`   | -128...127 |
|          | 16       | `short`   | -32,768...32,767 |
|          | 32       | `int`     | -2,147,483,648...2,147,483,647 |
|          | 64       | `long`    | -9,223,372,036,854,775,808...9,223,372,036,854,775,807 |
| 无符号整型 | 8        | `byte`    | 0...255 |
|          | 16       | `ushort`  | 0...65,535 |
|          | 32       | `uint`    | 0...4,294,967,295 |
|          | 64       | `ulong`   | 0...18,446,744,073,709,551,615 |
| 浮点型    | 32       | `float`   | 1.5 × 10^−45 to 3.4 × 10^38, 7位精度 |
|          | 64       | `double`  | 5.0 × 10^−324 to 1.7 × 10^308, 15位精度 |
| 小数型    | 128      | `decimal` | 1.0 × 10^−28 to 7.9 × 10^28, 28位精度 |

C#程序使用 ***类型声明*** 创建新类型。类型声明指定新类型的名称和成员。C#用户可定义的五种类型是：类，结构，接口，枚举和委托。

一个类的定义包含数据成员(字段)和函数成员(方法，属性等)等数据结构。类支持单继承和多态，这是派生类可以扩展和专门化基类的机制。

结构类型类似于类，因为它表示具有数据成员和函数成员的结构。但是，与类不同，结构是值类型，不需要堆的分配。结构类型不支持用户指定的继承，并且所有结构类型都隐式继承类型`object`。

接口类型将“合约”定义为一组命名的公共函数成员。实现接口的类或结构必须提供接口的函数成员的实现。接口可以继承自多个基接口，并且类或结构可以实现多个接口。

委托类型表示对具有特定参数列表和返回类型的方法的引用。委托使得可以将方法视为可以分配给变量并作为参数传递的实体。委托类似于在其他一些语言中找到的函数指针的概念，但与函数指针不同，委托是面向对象的，类型安全的。

类，结构，接口和委托类型都支持泛型，因此可以使用其他类型对它们进行参数化。

枚举类型是具有命名常量的不同类型。每个枚举类型都有一个底层类型，它必须是八种整数类型之一。枚举类型的值集与基础类型的值集相同。

C#支持任何类型的单维和多维数组。与上面列出的类型不同，数组类型在使用之前不必声明。而是通过使用带方括号的类型名称来构造数组类型。例如，`int []`是`int`的一维数组，`int [,]`是`int`的二维数组，`int [][]`是一维数组 `int[]`的一维数组。

使用可空类型之前也不必声明它们。对于每个非可空值类型`T`，有一个相应的可空类型`T？`，它可以保存一个附加值`null`。例如，`int？`是一种可以保存任何32位整数或值为`null`的类型。

C#的类型系统是统一的，任何类型的值都可以被视为一个对象。C#中的每个类型都直接或间接地派生自`object`类类型，而`object`是所有类型的最终基类。只需将值视`object`类型，即可将引用类型的值视为对象。通过执行 ***装箱*** 和 ***开箱*** 操作将即可值类型的值视为对象。在下面的示例中，`int`值转换为`object`并再次转换为`int`：

```csharp
using System;

class Test
{
    static void Main() {
        int i = 123;
        object o = i;          // 装箱
        int j = (int)o;        // 开箱
    }
}
```
当值类型的值转换为类型`object`时，将分配一个对象实例(也称为`箱子`)来保存该值，并将该值复制到该箱中。相反，当将`object`引用强制转换为值类型时，将检查引用的对象是否为正确值类型的箱，如果检查成功，则复制箱中的值。

C#的统一类型系统有效地意味着值类型可以“按需”成为对象。由于统一，使用类型`object`的通用库可以与引用类型和值类型一起使用。

C#中有几种 ***变量***，包括字段，数组元素，局部变量和参数。变量表示存储位置，每个变量都有一个类型，用于确定可以在变量中存储的值，如下表所示：


| __变量的类型__    | __可能的内容__ |
|----------|-----------------------|
| 不可空值类型 | 该确切类型的值 |
| 可空值类型 | 空值或该确切类型的值|
| `object` | 空引用，对任何引用类型的对象的引用，或对任何值类型的盒装值的引用|
| 类| 空引用，对该类类型实例的引用，或对从该类类型派生的类实例的引用|
| 接口| 空引用，对实现该接口类型的类类型实例的引用，或对实现该接口类型的值类型的盒装值的引用|
| 数组| 空引用，对该数组类型的实例的引用，或对兼容数组类型的实例的引用|
| 委托| 空引用或对该委托类型的实例的引用|

## 表达式

***表达式*** 由 ***运算对象*** 和 ***运算符*** 构成。表达式的运算符指示要应用于运算对象的运算。运算符的例子包括`+`，`-`，`*`，`/`和`new`。运算对象的示例包括文字，字段，局部变量和表达式。

当一个表达式包含有多种运算符时，运算符的 ***优先级*** 控制着各个运算符的计算顺序。例如，因为`*`的运算优先级高于`+`，表达式`x + y * z`运算结果与`x + (y * z)`相同。

大多数的运算符可以被 ***重载***。运算符重载允许为其中一个或两个运算对象是用户定义的类或结构类型的操作指定用户定义的运算符实现。

下表总结了C#的运算符，按从高到低的优先顺序列出了运算符类别。同一类别的运算符具有相同的优先权。


| __类别__                          | __表达式__         | __描述__        |
|----------------------------------|-------------------|-----------------|
| 主要                              | `x.m`             | 成员访问 |
|                                  | `x(...)`          | 方法和委托调用 |
|                                  | `x[...]`          | 数组和索引器访问 |
|                                  | `x++`             | 后递增 |
|                                  | `x--`             | 后递减 |
|                                  | `new T(...)`      | 对象与委托创建 |
|                                  | `new T(...){...}` | 使用初始化器创建对象 |
|                                  | `new {...}`       | 匿名对象初始化 |
|                                  | `new T[...]`      | 数组创建 |
|                                  | `typeof(T)`       | 包含`T`的`System.Type`对象 |
|                                  | `checked(x)`      | 在检查的上下文中计算表达式 |
|                                  | `unchecked(x)`    | 在未检查的上下文中计算表达式 |
|                                  | `default(T)`      | 获取类型`T`的默认值 |
|                                  | `delegate {...}`  | 匿名函数 (匿名方法) |
| 一元                              | `+x`              | 返回x的值 |
|                                  | `-x`              | 数值取反 |
|                                  | `!x`              | 逻辑取反 |
|                                  | `~x`              | 按位求补 |
|                                  | `++x`             | 前递增 |
|                                  | `--x`             | 前递减 |
|                                  | `(T)x`            | 显式地将`x`转换成类型`T` |
|                                  | `await x`         | 异步等待`x`完成 |
| 乘法                              | `x * y`           | 乘 |
|                                  | `x / y`           | 除 |
|                                  | `x % y`           | 求余 |
| 加减                              | `x + y`           | 相加，字符串串联，委托的组合 |
|                                  | `x - y`           | 相减, 委托删除 |
| 移位                              | `x << y`          | 左移位 |
|                                  | `x >> y`          | 右移位 |
| 关系和类型                         | `x < y`           | 小于 |
|                                  | `x > y`           | 大于 |
|                                  | `x <= y`          | 小于等于 |
|                                  | `x >= y`          | 大于等于 |
|                                  | `x is T`          | 如果`x`的类型是`T`，则返回`true`，否则返回`false` |
|                                  | `x as T`          | 如果`x`的类型是`T`，则返回`T`类型的`x`；否则返回`null` |
| 相等                              | `x == y`          | 相等      |
|                                  | `x != y`          | 不等 |
| 逻辑与                            | `x & y`           | 整数按位求与，布尔逻辑求与 |
| 逻辑异或                          | `x ^ y`           | 整数按位求异或，布尔逻辑求异或 |
| 逻辑或                            | `x | y`           | 整数按位求或，布尔逻辑求或 |
| 条件与                            | `x && y`          | 当且仅当`x`为`true`时计算`y`的值 |
| 条件或                            | `x || y`          | 当且仅当`x`为`false`时计算`y`的值 |
| Null 合并                         | `x ?? y`          | 当且仅当`x`为`null`时计算结果为`y`，否则计算结果为`x` |
| 条件运算                          | `x ? y : z`       | 如果`x`为`true`则赋值`y`，否则赋值`z` |
| 赋值或匿名函数                     | `x = y`           | 赋值 |
|                                  | `x op= y`         | 复合赋值；支持以下运算符： `*=` `/=` `%=` `+=` `-=` `<<=` `>>=` `&=` `^=` `|=` |
|                                  | `(T x) => y`      | 匿名函数 (lambda 表达式) |

## 语句

程序操作使用 ***语句*** 进行表示。C#支持几种不同的语句，其中许多语句是从嵌入语句的角度来定义的。

***代码块*** 允许编写一个语句的上下文中编写多个语句。代码块是由一系列在分隔符`{`和`}`内编写的语句组成。

***声明语句*** 用于声明局部变量和常量。

***表达式语句*** 用于计算表达式。可用作语句的表达式包括方法调用、使用运算符`new`的对象分配，使用`=`以及复合赋值运算符的赋值，使用运算符`++` 和`--`的递增和递减运算以及`await`表达式。

***选择语句*** 用于根据一些表达式的值从多个可能的语句中选择一个以供执行。这一类语句包括`if`语句和`switch`语句。

***迭代语句*** 用于重复执行嵌入语句。这一类语句包括`while`、`do`、`for`以及`foreach`语句。

***跳转语句*** 用于转移控制权。 这一类语句包括`break`、 `continue`、`goto`、`throw`、`return`以及`yield`语句。

`try`...`catch`语句用于捕获在代码块执行期间发生的异常，`try`...`finally`语句用于指定始终执行的最终代码，无论异常发生与否。

`checked`与`unchecked`语句用于控制整型类型算术运算和转换的溢出检查上下文。

`lock`语句用于获取给定对象的相互排斥锁定，执行语句，然后解除锁定。

`using`语句用于获取资源，执行语句，然后释放资源。

下面列出了各种可以使用的语句，以及每种语句的示例：

__局部变量声明__

```csharp
static void Main() {
   int a;
   int b = 2, c = 3;
   a = 1;
   Console.WriteLine(a + b + c);
}
```


__局部常量声明__

```csharp
static void Main() {
    const float pi = 3.1415927f;
    const int r = 25;
    Console.WriteLine(pi * r * r);
}
```


__表达式语句__

```csharp
static void Main() {
    int i;
    i = 123;                // 表达式语句
    Console.WriteLine(i);   // 表达式语句
    i++;                    // 表达式语句
    Console.WriteLine(i);   // 表达式语句
}
```

__`if`语句__

```csharp
static void Main(string[] args) {
    if (args.Length == 0) {
        Console.WriteLine("No arguments");
    }
    else {
        Console.WriteLine("One or more arguments");
    }
}
```


__`switch`语句__

```csharp
static void Main(string[] args) {
    int n = args.Length;
    switch (n) {
        case 0:
            Console.WriteLine("No arguments");
            break;
        case 1:
            Console.WriteLine("One argument");
            break;
        default:
            Console.WriteLine("{0} arguments", n);
            break;
    }
}
```

__`while`语句__

```csharp
static void Main(string[] args) {
    int i = 0;
    while (i < args.Length) {
        Console.WriteLine(args[i]);
        i++;
    }
}
```


__`do`语句__

```csharp
static void Main() {
    string s;
    do {
        s = Console.ReadLine();
        if (s != null) Console.WriteLine(s);
    } while (s != null);
}
```

__`for`语句__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        Console.WriteLine(args[i]);
    }
}
```

__`foreach`语句__

```csharp
static void Main(string[] args) {
    foreach (string s in args) {
        Console.WriteLine(s);
    }
}
```

__`break`语句__

```csharp
static void Main() {
    while (true) {
        string s = Console.ReadLine();
        if (s == null) break;
        Console.WriteLine(s);
    }
}
```

__`continue`语句__

```csharp
static void Main(string[] args) {
    for (int i = 0; i < args.Length; i++) {
        if (args[i].StartsWith("/")) continue;
        Console.WriteLine(args[i]);
    }
}
```

__`goto`语句__

```csharp
static void Main(string[] args) {
    int i = 0;
    goto check;
    loop:
    Console.WriteLine(args[i++]);
    check:
    if (i < args.Length) goto loop;
}
```

__`return`语句__

```csharp
static int Add(int a, int b) {
    return a + b;
}

static void Main() {
    Console.WriteLine(Add(1, 2));
    return;
}
```

__`yield`语句__

```csharp
static IEnumerable<int> Range(int from, int to) {
    for (int i = from; i < to; i++) {
        yield return i;
    }
    yield break;
}

static void Main() {
    foreach (int x in Range(-10,10)) {
        Console.WriteLine(x);
    }
}
```

__`throw`与`try`语句__

```csharp
static double Divide(double x, double y) {
    if (y == 0) throw new DivideByZeroException();
    return x / y;
}

static void Main(string[] args) {
    try {
        if (args.Length != 2) {
            throw new Exception("Two numbers required");
        }
        double x = double.Parse(args[0]);
        double y = double.Parse(args[1]);
        Console.WriteLine(Divide(x, y));
    }
    catch (Exception e) {
        Console.WriteLine(e.Message);
    }
    finally {
        Console.WriteLine("Good bye!");
    }
}
```

__`checked`与`unchecked`语句__

```csharp
static void Main() {
    int i = int.MaxValue;
    checked {
        Console.WriteLine(i + 1);        // 异常
    }
    unchecked {
        Console.WriteLine(i + 1);        // 溢出
    }
}
```

__`lock`语句__

```csharp
class Account
{
    decimal balance;
    public void Withdraw(decimal amount) {
        lock (this) {
            if (amount > balance) {
                throw new Exception("Insufficient funds");
            }
            balance -= amount;
        }
    }
}
```

__`using`语句__

```csharp
static void Main() {
    using (TextWriter w = File.CreateText("test.txt")) {
        w.WriteLine("Line one");
        w.WriteLine("Line two");
        w.WriteLine("Line three");
    }
}
```

## 类与对象

***类*** 是C#最基础的类型。类是一种将状态(字段)和动作(方法与其他函数成员)组合在一个单元中的数据结构。类提供了自动创建该类 ***实例*** (亦称 ***对象*** ) 的定义。类支持 ***继承*** 和 ***多态***， 即 ***派生类*** 可以扩展和专门化 ***基类*** 的机制。

新类使用类声明进行创建。 类声明的开头是标头，指定了类的特性和修饰符、类名、基类（若指定）以及类实现的接口。 标头后面是类主体，由在分隔符`{`和`}`内编写的成员声明列表组成。

以下是一个简单类`Point`的声明：

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
使用`new`运算符可创建类的实例，它为新实例分配内存，调用构造函数初始化实例，并返回对实例的引用。以下语句创建两个`Point`对象，并在两个变量中存储对这些对象的引用：

```csharp
Point p1 = new Point(0, 0);
Point p2 = new Point(10, 20);
```
当对象不再使用时，对象占用的内存将被自动回收。故在C#中显式地释放对象是不必要的，虽然并不能在C#中显式地释放对象。

### 成员

类的成员分为 ***静态成员*** 或 ***实例成员*** 。静态成员属于类，实例成员属于对象(类的实例)。

下表概述了类可以包含的成员类型：


| __成员__ | __描述__ |
|--------|-----------------|
| 常量    | 与类关联的常量值 |
| 字段    | 类的变量 |
| 方法    | 可以由类执行的计算和操作 |
| 属性    | 与读取和写入类的命名属性相关的操作 |
| 索引器  | 与索引类的实(如数组)相关联的操作 |
| 事件    | 可以由类生成的通知 |
| 运算符  | 类支持的转换和表达式运算符 |
| 构造函数 | 初始化类的实例或类本身所需的操作 |
| 析构函数 | 在类的实例被永久丢弃之前执行的操作 |
| 类型    | 类声明的嵌套类型 |

### 可访问性

类的每个成员都有一个对应的可访问性，它控制能够访问该成员的程序文本区域。可访问性有五种可能的形式，如下表所示：


|    __可访问性__       | __意义__ |
|----------------------|-----------------|
| `public`             | 访问不受限制 |
| `protected`          | 仅限于此类或从此类派生的类访问 |
| `internal`           | 仅限该程序集访问 |
| `protected internal` | 仅限该程序集与从此类派生的类访问 |
| `private`            | 仅限该类本身访问 |

### 类型参数

可以通过使用包含类型参数名称列表的尖括号跟随类名来指定一组类型参数定义类。类型参数可以在类声明的主体中使用，以定义类的成员。在下面的例子中，`TFirst`和`TSecond`是`Pair`的类型参数：

```csharp
public class Pair<TFirst,TSecond>
{
    public TFirst First;
    public TSecond Second;
}
```
被声明为采用类型参数的类被称为范型类。结构、接口和委托类型也能是范型类。

使用泛型类时，必须为每个类型参数提供类型参数：

```csharp
Pair<int,string> pair = new Pair<int,string> { First = 1, Second = "two" };
int i = pair.First;     // TFirst 是 int 型
string s = pair.Second; // TSecond 是 string 型
```
提供类型参数的泛型类型，如上面的`Pair <int，string>`，称为构造类型。

### 基类

声明类时可以通过在该类名称和类型参数后面添加冒号与其他类名来为该类指定一个基类型。省略基类指定等价于该类从`object`类型派生。在下面的例子中，类`Point3D`的基类是`Point`类，而类`Point`的基类是`object`:

```csharp
public class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

public class Point3D: Point
{
    public int z;

    public Point3D(int x, int y, int z): base(x, y) {
        this.z = z;
    }
}
```
派生类将会继承基类的成员。继承代表一个类除了实例和静态构造函数，以及析构函数外隐式地含有它基类的所有成员。派生类可以给它所继承的类添加成员，但不能移除任何一个继承来的成员。在前面的例子中，类`Point3D`从`Point`类继承了字段`x`和`y`，并且每一个`Point3D`实例包含有三个字段`x`、 `y`、`z`。

任何类与其基类之间存在着隐式转换。故某类的变量可引用该类以及该类的派生类的实例。例如，在有上述类的声明的前提下，一个`Point`类型的变量可以引用`Point`或`Point3D`类型的实例：

```csharp
Point a = new Point(10, 20);
Point b = new Point3D(10, 20, 30);
```

### 字段

字段是与某类或某类实例相关的变量。

使用`static`修饰符声明的字段称作 ***静态字段***。静态字段只标记一个存储位置，无论创建了多少个某类的实例，对于该类的某个静态字段永远只有一个拷贝。

A field declared without the `static` modifier defines an ***instance field***. Every instance of a class contains a separate copy of all the instance fields of that class.

In the following example, each instance of the `Color` class has a separate copy of the `r`, `g`, and `b` instance fields, but there is only one copy of the `Black`, `White`, `Red`, `Green`, and `Blue` static fields:

```csharp
public class Color
{
    public static readonly Color Black = new Color(0, 0, 0);
    public static readonly Color White = new Color(255, 255, 255);
    public static readonly Color Red = new Color(255, 0, 0);
    public static readonly Color Green = new Color(0, 255, 0);
    public static readonly Color Blue = new Color(0, 0, 255);
    private byte r, g, b;

    public Color(byte r, byte g, byte b) {
        this.r = r;
        this.g = g;
        this.b = b;
    }
}
```
As shown in the previous example, ***read-only fields*** may be declared with a `readonly` modifier. Assignment to a `readonly` field can only occur as part of the field's declaration or in a constructor in the same class.

### Methods

A ***method*** is a member that implements a computation or action that can be performed by an object or class. ***Static methods*** are accessed through the class. ***Instance methods*** are accessed through instances of the class.

Methods have a (possibly empty) list of ***parameters***, which represent values or variable references passed to the method, and a ***return type***, which specifies the type of the value computed and returned by the method. A method's return type is `void` if it does not return a value.

Like types, methods may also have a set of type parameters, for which type arguments must be specified when the method is called. Unlike types, the type arguments can often be inferred from the arguments of a method call and need not be explicitly given.

The ***signature*** of a method must be unique in the class in which the method is declared. The signature of a method consists of the name of the method, the number of type parameters and the number, modifiers, and types of its parameters. The signature of a method does not include the return type.

#### Parameters

Parameters are used to pass values or variable references to methods. The parameters of a method get their actual values from the ***arguments*** that are specified when the method is invoked. There are four kinds of parameters: value parameters, reference parameters, output parameters, and parameter arrays.

A ***value parameter*** is used for input parameter passing. A value parameter corresponds to a local variable that gets its initial value from the argument that was passed for the parameter. Modifications to a value parameter do not affect the argument that was passed for the parameter.

Value parameters can be optional, by specifying a default value so that corresponding arguments can be omitted.

A ***reference parameter*** is used for both input and output parameter passing. The argument passed for a reference parameter must be a variable, and during execution of the method, the reference parameter represents the same storage location as the argument variable. A reference parameter is declared with the `ref` modifier. The following example shows the use of `ref` parameters.

```csharp
using System;

class Test
{
    static void Swap(ref int x, ref int y) {
        int temp = x;
        x = y;
        y = temp;
    }

    static void Main() {
        int i = 1, j = 2;
        Swap(ref i, ref j);
        Console.WriteLine("{0} {1}", i, j);            // Outputs "2 1"
    }
}
```
An ***output parameter*** is used for output parameter passing. An output parameter is similar to a reference parameter except that the initial value of the caller-provided argument is unimportant. An output parameter is declared with the `out` modifier. The following example shows the use of `out` parameters.

```csharp
using System;

class Test
{
    static void Divide(int x, int y, out int result, out int remainder) {
        result = x / y;
        remainder = x % y;
    }

    static void Main() {
        int res, rem;
        Divide(10, 3, out res, out rem);
        Console.WriteLine("{0} {1}", res, rem);    // Outputs "3 1"
    }
}
```
A ***parameter array*** permits a variable number of arguments to be passed to a method. A parameter array is declared with the `params` modifier. Only the last parameter of a method can be a parameter array, and the type of a parameter array must be a single-dimensional array type. The `Write` and `WriteLine` methods of the `System.Console` class are good examples of parameter array usage. They are declared as follows.

```csharp
public class Console
{
    public static void Write(string fmt, params object[] args) {...}
    public static void WriteLine(string fmt, params object[] args) {...}
    ...
}
```
Within a method that uses a parameter array, the parameter array behaves exactly like a regular parameter of an array type. However, in an invocation of a method with a parameter array, it is possible to pass either a single argument of the parameter array type or any number of arguments of the element type of the parameter array. In the latter case, an array instance is automatically created and initialized with the given arguments. This example

```csharp
Console.WriteLine("x={0} y={1} z={2}", x, y, z);
```
is equivalent to writing the following.

```csharp
string s = "x={0} y={1} z={2}";
object[] args = new object[3];
args[0] = x;
args[1] = y;
args[2] = z;
Console.WriteLine(s, args);
```

#### Method body and local variables

A method's body specifies the statements to execute when the method is invoked.

A method body can declare variables that are specific to the invocation of the method. Such variables are called ***local variables***. A local variable declaration specifies a type name, a variable name, and possibly an initial value. The following example declares a local variable `i` with an initial value of zero and a local variable `j` with no initial value.

```csharp
using System;

class Squares
{
    static void Main() {
        int i = 0;
        int j;
        while (i < 10) {
            j = i * i;
            Console.WriteLine("{0} x {0} = {1}", i, j);
            i = i + 1;
        }
    }
}
```
C# requires a local variable to be ***definitely assigned*** before its value can be obtained. For example, if the declaration of the previous `i` did not include an initial value, the compiler would report an error for the subsequent usages of `i` because `i` would not be definitely assigned at those points in the program.

A method can use `return` statements to return control to its caller. In a method returning `void`, `return` statements cannot specify an expression. In a method returning non-`void`, `return` statements must include an expression that computes the return value.

#### Static and instance methods

A method declared with a `static` modifier is a ***static method***. A static method does not operate on a specific instance and can only directly access static members.

A method declared without a `static` modifier is an ***instance method***. An instance method operates on a specific instance and can access both static and instance members. The instance on which an instance method was invoked can be explicitly accessed as `this`. It is an error to refer to `this` in a static method.

The following `Entity` class has both static and instance members.

```csharp
class Entity
{
    static int nextSerialNo;
    int serialNo;

    public Entity() {
        serialNo = nextSerialNo++;
    }

    public int GetSerialNo() {
        return serialNo;
    }

    public static int GetNextSerialNo() {
        return nextSerialNo;
    }

    public static void SetNextSerialNo(int value) {
        nextSerialNo = value;
    }
}
```
Each `Entity` instance contains a serial number (and presumably some other information that is not shown here). The `Entity` constructor (which is like an instance method) initializes the new instance with the next available serial number. Because the constructor is an instance member, it is permitted to access both the `serialNo` instance field and the `nextSerialNo` static field.

The `GetNextSerialNo` and `SetNextSerialNo` static methods can access the `nextSerialNo` static field, but it would be an error for them to directly access the `serialNo` instance field.

The following example shows the use of the `Entity` class.

```csharp
using System;

class Test
{
    static void Main() {
        Entity.SetNextSerialNo(1000);
        Entity e1 = new Entity();
        Entity e2 = new Entity();
        Console.WriteLine(e1.GetSerialNo());           // Outputs "1000"
        Console.WriteLine(e2.GetSerialNo());           // Outputs "1001"
        Console.WriteLine(Entity.GetNextSerialNo());   // Outputs "1002"
    }
}
```
Note that the `SetNextSerialNo` and `GetNextSerialNo` static methods are invoked on the class whereas the `GetSerialNo` instance method is invoked on instances of the class.

#### Virtual, override, and abstract methods

When an instance method declaration includes a `virtual` modifier, the method is said to be a ***virtual method***. When no `virtual` modifier is present, the method is said to be a ***non-virtual method***.

When a virtual method is invoked, the ***run-time type*** of the instance for which that invocation takes place determines the actual method implementation to invoke. In a nonvirtual method invocation, the ***compile-time type*** of the instance is the determining factor.

A virtual method can be ***overridden*** in a derived class. When an instance method declaration includes an `override` modifier, the method overrides an inherited virtual method with the same signature. Whereas a virtual method declaration introduces a new method, an override method declaration specializes an existing inherited virtual method by providing a new implementation of that method.

An ***abstract*** method is a virtual method with no implementation. An abstract method is declared with the `abstract` modifier and is permitted only in a class that is also declared `abstract`. An abstract method must be overridden in every non-abstract derived class.

The following example declares an abstract class, `Expression`, which represents an expression tree node, and three derived classes, `Constant`, `VariableReference`, and `Operation`, which implement expression tree nodes for constants, variable references, and arithmetic operations. (This is similar to, but not to be confused with the expression tree types introduced in [Expression tree types](types.md#expression-tree-types)).

```csharp
using System;
using System.Collections;

public abstract class Expression
{
    public abstract double Evaluate(Hashtable vars);
}

public class Constant: Expression
{
    double value;

    public Constant(double value) {
        this.value = value;
    }

    public override double Evaluate(Hashtable vars) {
        return value;
    }
}

public class VariableReference: Expression
{
    string name;

    public VariableReference(string name) {
        this.name = name;
    }

    public override double Evaluate(Hashtable vars) {
        object value = vars[name];
        if (value == null) {
            throw new Exception("Unknown variable: " + name);
        }
        return Convert.ToDouble(value);
    }
}

public class Operation: Expression
{
    Expression left;
    char op;
    Expression right;

    public Operation(Expression left, char op, Expression right) {
        this.left = left;
        this.op = op;
        this.right = right;
    }

    public override double Evaluate(Hashtable vars) {
        double x = left.Evaluate(vars);
        double y = right.Evaluate(vars);
        switch (op) {
            case '+': return x + y;
            case '-': return x - y;
            case '*': return x * y;
            case '/': return x / y;
        }
        throw new Exception("Unknown operator");
    }
}
```
The previous four classes can be used to model arithmetic expressions. For example, using instances of these classes, the expression `x + 3` can be represented as follows.

```csharp
Expression e = new Operation(
    new VariableReference("x"),
    '+',
    new Constant(3));
```
The `Evaluate` method of an `Expression` instance is invoked to evaluate the given expression and produce a `double` value. The method takes as an argument a `Hashtable` that contains variable names (as keys of the entries) and values (as values of the entries). The `Evaluate` method is a virtual abstract method, meaning that non-abstract derived classes must override it to provide an actual implementation.

A `Constant`'s implementation of `Evaluate` simply returns the stored constant. A `VariableReference`'s implementation looks up the variable name in the hashtable and returns the resulting value. An `Operation`'s implementation first evaluates the left and right operands (by recursively invoking their `Evaluate` methods) and then performs the given arithmetic operation.

The following program uses the `Expression` classes to evaluate the expression `x * (y + 2)` for different values of `x` and `y`.

```csharp
using System;
using System.Collections;

class Test
{
    static void Main() {
        Expression e = new Operation(
            new VariableReference("x"),
            '*',
            new Operation(
                new VariableReference("y"),
                '+',
                new Constant(2)
            )
        );
        Hashtable vars = new Hashtable();
        vars["x"] = 3;
        vars["y"] = 5;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "21"
        vars["x"] = 1.5;
        vars["y"] = 9;
        Console.WriteLine(e.Evaluate(vars));        // Outputs "16.5"
    }
}
```

#### Method overloading

Method ***overloading*** permits multiple methods in the same class to have the same name as long as they have unique signatures. When compiling an invocation of an overloaded method, the compiler uses ***overload resolution*** to determine the specific method to invoke. Overload resolution finds the one method that best matches the arguments or reports an error if no single best match can be found. The following example shows overload resolution in effect. The comment for each invocation in the `Main` method shows which method is actually invoked.

```csharp
class Test
{
    static void F() {
        Console.WriteLine("F()");
    }

    static void F(object x) {
        Console.WriteLine("F(object)");
    }

    static void F(int x) {
        Console.WriteLine("F(int)");
    }

    static void F(double x) {
        Console.WriteLine("F(double)");
    }

    static void F<T>(T x) {
        Console.WriteLine("F<T>(T)");
    }

    static void F(double x, double y) {
        Console.WriteLine("F(double, double)");
    }

    static void Main() {
        F();                 // Invokes F()
        F(1);                // Invokes F(int)
        F(1.0);              // Invokes F(double)
        F("abc");            // Invokes F(object)
        F((double)1);        // Invokes F(double)
        F((object)1);        // Invokes F(object)
        F<int>(1);           // Invokes F<T>(T)
        F(1, 1);             // Invokes F(double, double)
    }
}
```
As shown by the example, a particular method can always be selected by explicitly casting the arguments to the exact parameter types and/or explicitly supplying type arguments.

### Other function members

Members that contain executable code are collectively known as the ***function members*** of a class. The preceding section describes methods, which are the primary kind of function members. This section describes the other kinds of function members supported by C#: constructors, properties, indexers, events, operators, and destructors.

The following code shows a generic class called `List<T>`, which implements a growable list of objects. The class contains several examples of the most common kinds of function members.


```csharp
public class List<T> {
    // Constant...
    const int defaultCapacity = 4;

    // Fields...
    T[] items;
    int count;

    // Constructors...
    public List(int capacity = defaultCapacity) {
        items = new T[capacity];
    }

    // Properties...
    public int Count {
        get { return count; }
    }
    public int Capacity {
        get {
            return items.Length;
        }
        set {
            if (value < count) value = count;
            if (value != items.Length) {
                T[] newItems = new T[value];
                Array.Copy(items, 0, newItems, 0, count);
                items = newItems;
            }
        }
    }

    // Indexer...
    public T this[int index] {
        get {
            return items[index];
        }
        set {
            items[index] = value;
            OnChanged();
        }
    }

    // Methods...
    public void Add(T item) {
        if (count == Capacity) Capacity = count * 2;
        items[count] = item;
        count++;
        OnChanged();
    }
    protected virtual void OnChanged() {
        if (Changed != null) Changed(this, EventArgs.Empty);
    }
    public override bool Equals(object other) {
        return Equals(this, other as List<T>);
    }
    static bool Equals(List<T> a, List<T> b) {
        if (a == null) return b == null;
        if (b == null || a.count != b.count) return false;
        for (int i = 0; i < a.count; i++) {
            if (!object.Equals(a.items[i], b.items[i])) {
                return false;
            }
        }
        return true;
    }

    // Event...
    public event EventHandler Changed;

    // Operators...
    public static bool operator ==(List<T> a, List<T> b) {
        return Equals(a, b);
    }
    public static bool operator !=(List<T> a, List<T> b) {
        return !Equals(a, b);
    }
}
```

#### Constructors

C# supports both instance and static constructors. An ***instance constructor*** is a member that implements the actions required to initialize an instance of a class. A ***static constructor*** is a member that implements the actions required to initialize a class itself when it is first loaded.

A constructor is declared like a method with no return type and the same name as the containing class. If a constructor declaration includes a `static` modifier, it declares a static constructor. Otherwise, it declares an instance constructor.

Instance constructors can be overloaded. For example, the `List<T>
` class declares two instance constructors, one with no parameters and one that takes an `int` parameter. Instance constructors are invoked using the `new` operator. The following statements allocate two `List<string>
` instances using each of the constructors of the `List` class.

```csharp
List<string> list1 = new List<string>();
List<string> list2 = new List<string>(10);
```
Unlike other members, instance constructors are not inherited, and a class has no instance constructors other than those actually declared in the class. If no instance constructor is supplied for a class, then an empty one with no parameters is automatically provided.

#### Properties

***Properties*** are a natural extension of fields. Both are named members with associated types, and the syntax for accessing fields and properties is the same. However, unlike fields, properties do not denote storage locations. Instead, properties have ***accessors*** that specify the statements to be executed when their values are read or written.

A property is declared like a field, except that the declaration ends with a `get` accessor and/or a `set` accessor written between the delimiters `{` and `}` instead of ending in a semicolon. A property that has both a `get` accessor and a `set` accessor is a ***read-write property***, a property that has only a `get` accessor is a ***read-only property***, and a property that has only a `set` accessor is a ***write-only property***.

A `get` accessor corresponds to a parameterless method with a return value of the property type. Except as the target of an assignment, when a property is referenced in an expression, the `get` accessor of the property is invoked to compute the value of the property.

A `set` accessor corresponds to a method with a single parameter named `value` and no return type. When a property is referenced as the target of an assignment or as the operand of `++` or `--`, the `set` accessor is invoked with an argument that provides the new value.

The `List<T>
` class declares two properties, `Count` and `Capacity`, which are read-only and read-write, respectively. The following is an example of use of these properties.

```csharp
List<string> names = new List<string>();
names.Capacity = 100;            // Invokes set accessor
int i = names.Count;             // Invokes get accessor
int j = names.Capacity;          // Invokes get accessor
```
Similar to fields and methods, C# supports both instance properties and static properties. Static properties are declared with the `static` modifier, and instance properties are declared without it.

The accessor(s) of a property can be virtual. When a property declaration includes a `virtual`, `abstract`, or `override` modifier, it applies to the accessor(s) of the property.

#### Indexers

An ***indexer*** is a member that enables objects to be indexed in the same way as an array. An indexer is declared like a property except that the name of the member is `this` followed by a parameter list written between the delimiters `[` and `]`. The parameters are available in the accessor(s) of the indexer. Similar to properties, indexers can be read-write, read-only, and write-only, and the accessor(s) of an indexer can be virtual.

The `List` class declares a single read-write indexer that takes an `int` parameter. The indexer makes it possible to index `List` instances with `int` values. For example

```csharp
List<string> names = new List<string>();
names.Add("Liz");
names.Add("Martha");
names.Add("Beth");
for (int i = 0; i < names.Count; i++) {
    string s = names[i];
    names[i] = s.ToUpper();
}
```
Indexers can be overloaded, meaning that a class can declare multiple indexers as long as the number or types of their parameters differ.

#### Events

An ***event*** is a member that enables a class or object to provide notifications. An event is declared like a field except that the declaration includes an `event` keyword and the type must be a delegate type.

Within a class that declares an event member, the event behaves just like a field of a delegate type (provided the event is not abstract and does not declare accessors). The field stores a reference to a delegate that represents the event handlers that have been added to the event. If no event handles are present, the field is `null`.

The `List<T>
` class declares a single event member called `Changed`, which indicates that a new item has been added to the list. The `Changed` event is raised by the `OnChanged` virtual method, which first checks whether the event is `null` (meaning that no handlers are present). The notion of raising an event is precisely equivalent to invoking the delegate represented by the event—thus, there are no special language constructs for raising events.

Clients react to events through ***event handlers***. Event handlers are attached using the `+=` operator and removed using the `-=` operator. The following example attaches an event handler to the `Changed` event of a `List<string>
`.

```csharp
using System;

class Test
{
    static int changeCount;

    static void ListChanged(object sender, EventArgs e) {
        changeCount++;
    }

    static void Main() {
        List<string> names = new List<string>();
        names.Changed += new EventHandler(ListChanged);
        names.Add("Liz");
        names.Add("Martha");
        names.Add("Beth");
        Console.WriteLine(changeCount);        // Outputs "3"
    }
}
```
For advanced scenarios where control of the underlying storage of an event is desired, an event declaration can explicitly provide `add` and `remove` accessors, which are somewhat similar to the `set` accessor of a property.

#### Operators

An ***operator*** is a member that defines the meaning of applying a particular expression operator to instances of a class. Three kinds of operators can be defined: unary operators, binary operators, and conversion operators. All operators must be declared as `public` and `static`.

The `List<T>
` class declares two operators, `operator==` and `operator!=`, and thus gives new meaning to expressions that apply those operators to `List` instances. Specifically, the operators define equality of two `List<T>
` instances as comparing each of the contained objects using their `Equals` methods. The following example uses the `==` operator to compare two `List<int>
` instances.

```csharp
using System;

class Test
{
    static void Main() {
        List<int> a = new List<int>();
        a.Add(1);
        a.Add(2);
        List<int> b = new List<int>();
        b.Add(1);
        b.Add(2);
        Console.WriteLine(a == b);        // Outputs "True"
        b.Add(3);
        Console.WriteLine(a == b);        // Outputs "False"
    }
}
```

The first `Console.WriteLine` outputs `True` because the two lists contain the same number of objects with the same values in the same order. Had `List<T>
` not defined `operator==`, the first `Console.WriteLine` would have output `False` because `a` and `b` reference different `List<int>
` instances.

#### Destructors

A ***destructor*** is a member that implements the actions required to destruct an instance of a class. Destructors cannot have parameters, they cannot have accessibility modifiers, and they cannot be invoked explicitly. The destructor for an instance is invoked automatically during garbage collection.

The garbage collector is allowed wide latitude in deciding when to collect objects and run destructors. Specifically, the timing of destructor invocations is not deterministic, and destructors may be executed on any thread. For these and other reasons, classes should implement destructors only when no other solutions are feasible.

The `using` statement provides a better approach to object destruction.

## Structs

Like classes, ***structs*** are data structures that can contain data members and function members, but unlike classes, structs are value types and do not require heap allocation. A variable of a struct type directly stores the data of the struct, whereas a variable of a class type stores a reference to a dynamically allocated object. Struct types do not support user-specified inheritance, and all struct types implicitly inherit from type `object`.

Structs are particularly useful for small data structures that have value semantics. Complex numbers, points in a coordinate system, or key-value pairs in a dictionary are all good examples of structs. The use of structs rather than classes for small data structures can make a large difference in the number of memory allocations an application performs. For example, the following program creates and initializes an array of 100 points. With `Point` implemented as a class, 101 separate objects are instantiated—one for the array and one each for the 100 elements.

```csharp
class Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

class Test
{
    static void Main() {
        Point[] points = new Point[100];
        for (int i = 0; i < 100; i++) points[i] = new Point(i, i);
    }
}
```
An alternative is to make `Point` a struct.

```csharp
struct Point
{
    public int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```
Now, only one object is instantiated—the one for the array—and the `Point` instances are stored in-line in the array.

Struct constructors are invoked with the `new` operator, but that does not imply that memory is being allocated. Instead of dynamically allocating an object and returning a reference to it, a struct constructor simply returns the struct value itself (typically in a temporary location on the stack), and this value is then copied as necessary.

With classes, it is possible for two variables to reference the same object and thus possible for operations on one variable to affect the object referenced by the other variable. With structs, the variables each have their own copy of the data, and it is not possible for operations on one to affect the other. For example, the output produced by the following code fragment depends on whether `Point` is a class or a struct.

```csharp
Point a = new Point(10, 10);
Point b = a;
a.x = 20;
Console.WriteLine(b.x);
```
If `Point` is a class, the output is `20` because `a` and `b` reference the same object. If `Point` is a struct, the output is `10` because the assignment of `a` to `b` creates a copy of the value, and this copy is unaffected by the subsequent assignment to `a.x`.

The previous example highlights two of the limitations of structs. First, copying an entire struct is typically less efficient than copying an object reference, so assignment and value parameter passing can be more expensive with structs than with reference types. Second, except for `ref` and `out` parameters, it is not possible to create references to structs, which rules out their usage in a number of situations.

## Arrays

An ***array*** is a data structure that contains a number of variables that are accessed through computed indices. The variables contained in an array, also called the ***elements*** of the array, are all of the same type, and this type is called the ***element type*** of the array.

Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance. Actual array instances are created dynamically at run-time using the `new` operator. The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance. The indices of the elements of an array range from `0` to `Length - 1`. The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.

The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.

```csharp
using System;

class Test
{
    static void Main() {
        int[] a = new int[10];
        for (int i = 0; i < a.Length; i++) {
            a[i] = i * i;
        }
        for (int i = 0; i < a.Length; i++) {
            Console.WriteLine("a[{0}] = {1}", i, a[i]);
        }
    }
}
```
This example creates and operates on a ***single-dimensional array***. C# also supports ***multi-dimensional arrays***. The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type. The following example allocates a one-dimensional, a two-dimensional, and a three-dimensional array.

```csharp
int[] a1 = new int[10];
int[,] a2 = new int[10, 5];
int[,,] a3 = new int[10, 5, 2];
```
The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.

The element type of an array can be any type, including an array type. An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays do not all have to be the same. The following example allocates an array of arrays of `int`:

```csharp
int[][] a = new int[3][];
a[0] = new int[10];
a[1] = new int[5];
a[2] = new int[20];
```
The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`. The subsequent lines then initialize the three elements with references to individual array instances of varying lengths.

The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`. The following example allocates and initializes an `int[]` with three elements.

```csharp
int[] a = new int[] {1, 2, 3};
```
Note that the length of the array is inferred from the number of expressions between `{` and `}`. Local variable and field declarations can be shortened further such that the array type does not have to be restated.

```csharp
int[] a = {1, 2, 3};
```
Both of the previous examples are equivalent to the following:

```csharp
int[] t = new int[3];
t[0] = 1;
t[1] = 2;
t[2] = 3;
int[] a = t;
```
## Interfaces

An ***interface*** defines a contract that can be implemented by classes and structs. An interface can contain methods, properties, events, and indexers. An interface does not provide implementations of the members it defines—it merely specifies the members that must be supplied by classes or structs that implement the interface.

Interfaces may employ ***multiple inheritance***. In the following example, the interface `IComboBox` inherits from both `ITextBox` and `IListBox`.

```csharp
interface IControl
{
    void Paint();
}

interface ITextBox: IControl
{
    void SetText(string text);
}

interface IListBox: IControl
{
    void SetItems(string[] items);
}

interface IComboBox: ITextBox, IListBox {}
```
Classes and structs can implement multiple interfaces. In the following example, the class `EditBox` implements both `IControl` and `IDataBound`.

```csharp
interface IDataBound
{
    void Bind(Binder b);
}

public class EditBox: IControl, IDataBound
{
    public void Paint() {...}
    public void Bind(Binder b) {...}
}
```
When a class or struct implements a particular interface, instances of that class or struct can be implicitly converted to that interface type. For example

```csharp
EditBox editBox = new EditBox();
IControl control = editBox;
IDataBound dataBound = editBox;
```
In cases where an instance is not statically known to implement a particular interface, dynamic type casts can be used. For example, the following statements use dynamic type casts to obtain an object's `IControl` and `IDataBound` interface implementations. Because the actual type of the object is `EditBox`, the casts succeed.

```csharp
object obj = new EditBox();
IControl control = (IControl)obj;
IDataBound dataBound = (IDataBound)obj;
```
In the previous `EditBox` class, the `Paint` method from the `IControl` interface and the `Bind` method from the `IDataBound` interface are implemented using `public` members. C# also supports ***explicit interface member implementations***, using which the class or struct can avoid making the members `public`. An explicit interface member implementation is written using the fully qualified interface member name. For example, the `EditBox` class could implement the `IControl.Paint` and `IDataBound.Bind` methods using explicit interface member implementations as follows.

```csharp
public class EditBox: IControl, IDataBound
{
    void IControl.Paint() {...}
    void IDataBound.Bind(Binder b) {...}
}
```
Explicit interface members can only be accessed via the interface type. For example, the implementation of `IControl.Paint` provided by the previous `EditBox` class can only be invoked by first converting the `EditBox` reference to the `IControl` interface type.

```csharp
EditBox editBox = new EditBox();
editBox.Paint();                        // Error, no such method
IControl control = editBox;
control.Paint();                        // Ok
```

## Enums

An ***enum type*** is a distinct value type with a set of named constants. The following example declares and uses an enum type named `Color` with three constant values, `Red`, `Green`, and `Blue`.

```csharp
using System;

enum Color
{
    Red,
    Green,
    Blue
}

class Test
{
    static void PrintColor(Color color) {
        switch (color) {
            case Color.Red:
                Console.WriteLine("Red");
                break;
            case Color.Green:
                Console.WriteLine("Green");
                break;
            case Color.Blue:
                Console.WriteLine("Blue");
                break;
            default:
                Console.WriteLine("Unknown color");
                break;
        }
    }

    static void Main() {
        Color c = Color.Red;
        PrintColor(c);
        PrintColor(Color.Blue);
    }
}
```
Each enum type has a corresponding integral type called the ***underlying type*** of the enum type. An enum type that does not explicitly declare an underlying type has an underlying type of `int`. An enum type's storage format and range of possible values are determined by its underlying type. The set of values that an enum type can take on is not limited by its enum members. In particular, any value of the underlying type of an enum can be cast to the enum type and is a distinct valid value of that enum type.

The following example declares an enum type named `Alignment` with an underlying type of `sbyte`.

```csharp
enum Alignment: sbyte
{
    Left = -1,
    Center = 0,
    Right = 1
}
```
As shown by the previous example, an enum member declaration can include a constant expression that specifies the value of the member. The constant value for each enum member must be in the range of the underlying type of the enum. When an enum member declaration does not explicitly specify a value, the member is given the value zero (if it is the first member in the enum type) or the value of the textually preceding enum member plus one.

Enum values can be converted to integral values and vice versa using type casts. For example

```csharp
int i = (int)Color.Blue;        // int i = 2;
Color c = (Color)2;             // Color c = Color.Blue;
```
The default value of any enum type is the integral value zero converted to the enum type. In cases where variables are automatically initialized to a default value, this is the value given to variables of enum types. In order for the default value of an enum type to be easily available, the literal `0` implicitly converts to any enum type. Thus, the following is permitted.

```csharp
Color c = 0;
```

## Delegates

A ***delegate type*** represents references to methods with a particular parameter list and return type. Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters. Delegates are similar to the concept of function pointers found in some other languages, but unlike function pointers, delegates are object-oriented and type-safe.

The following example declares and uses a delegate type named `Function`.

```csharp
using System;

delegate double Function(double x);

class Multiplier
{
    double factor;

    public Multiplier(double factor) {
        this.factor = factor;
    }

    public double Multiply(double x) {
        return x * factor;
    }
}

class Test
{
    static double Square(double x) {
        return x * x;
    }

    static double[] Apply(double[] a, Function f) {
        double[] result = new double[a.Length];
        for (int i = 0; i < a.Length; i++) result[i] = f(a[i]);
        return result;
    }

    static void Main() {
        double[] a = {0.0, 0.5, 1.0};
        double[] squares = Apply(a, Square);
        double[] sines = Apply(a, Math.Sin);
        Multiplier m = new Multiplier(2.0);
        double[] doubles =  Apply(a, m.Multiply);
    }
}
```
An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value. The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results. In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.

A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example). A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.

Delegates can also be created using anonymous functions, which are "inline methods" that are created on the fly. Anonymous functions can see the local variables of the surrounding methods. Thus, the multiplier example above can be written more easily without using a `Multiplier` class:

```csharp
double[] doubles =  Apply(a, (double x) => x * 2.0);
```
An interesting and useful property of a delegate is that it does not know or care about the class of the method it references; all that matters is that the referenced method has the same parameters and return type as the delegate.

## Attributes

Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior. For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers. C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time. Programs specify this additional declarative information by defining and using ***attributes***.

The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.

```csharp
using System;

public class HelpAttribute: Attribute
{
    string url;
    string topic;

    public HelpAttribute(string url) {
        this.url = url;
    }

    public string Url {
        get { return url; }
    }

    public string Topic {
        get { return topic; }
        set { topic = value; }
    }
}
```
All attribute classes derive from the `System.Attribute` base class provided by the .NET Framework. Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration. If an attribute's name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced. For example, the `HelpAttribute` attribute can be used as follows.

```csharp
[Help("http://msdn.microsoft.com/.../MyClass.htm")]
public class Widget
{
    [Help("http://msdn.microsoft.com/.../MyClass.htm", Topic = "Display")]
    public void Display(string text) {}
}
```
This example attaches a `HelpAttribute` to the `Widget` class and another `HelpAttribute` to the `Display` method in the class. The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity. Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).

The following example shows how attribute information for a given program entity can be retrieved at run-time using reflection.

```csharp
using System;
using System.Reflection;

class Test
{
    static void ShowHelp(MemberInfo member) {
        HelpAttribute a = Attribute.GetCustomAttribute(member,
            typeof(HelpAttribute)) as HelpAttribute;
        if (a == null) {
            Console.WriteLine("No help for {0}", member);
        }
        else {
            Console.WriteLine("Help for {0}:", member);
            Console.WriteLine("  Url={0}, Topic={1}", a.Url, a.Topic);
        }
    }

    static void Main() {
        ShowHelp(typeof(Widget));
        ShowHelp(typeof(Widget).GetMethod("Display"));
    }
}
```
When a particular attribute is requested through reflection, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned. If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.
