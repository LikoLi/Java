# 第六章 初始化和清理

## 利用构造器保证初始化

在Java中，类的设计者通过构造器保证每个对象的初始化。如果一个类有构造器，那么 Java 会在用户使用对象之前（即对象刚创建完成）自动调用对象的构造器方法，从而保证初始化。

构造器名称同类名相同（构造器方法名与类名相同，不需要符合首字母小写的编程风格）

当创建一个对象是，内存被分配，构造器被调用。构造器保证了对象在你使用它之前进行了正确的初始化。

构造器没有返回值，它是一种特殊的方法。但它和返回类型为 `void` 的普通方法不同，普通方法可以返回空值，你还能选择让它返回别的类型；而构造器没有返回值，却同时也没有给你选择的余地

## 方法重载

命名

大多数编程语言（尤其是 C 语言）要求为每个方法（在这些语言中经常称为函数）提供一个独一无二的标识符。

方法重载是必要的，它允许方法具有相同的方法名但接收的参数不同。尽管方法重载对于构造器是重要的，但是也可以对任何方法很方便地进行重载。

### 区分重载方法

每个被重载的方法必须有独一无二的参数列表（构造器需要重载但是没有参数列表，所以标识符中不应该有返回值）

### 重载与基本类型

基本类型可以自动从较小的类型转型为较大的类型。

如果传入的参数类型大于方法期望接收的参数类型，你必须首先做下转换，如果你不做的话，编译器就会报错。

### 返回值的重载

调用一个方法且忽略返回值。这叫做调用一个函数的副作用，因为你不在乎返回值，只是想利用方法做些事。所以如果你直接调用 `f()`，Java 编译器就不知道你想调用哪个方法，阅读者也不明所以。因为这个原因，所以你不能根据返回值类型区分重载的方法。为了支持新特性，Java 8 在一些具体情形下提高了猜测的准确度，但是通常来说并不起作用。

## 无参构造器

一个无参构造器就是不接收参数的构造器，用来创建一个"默认的对象"。如果你创建一个类，类中没有构造器，那么编译器就会自动为你创建一个无参构造器。但是,一旦你显式地定义了构造器（无论有参还是无参），编译器就不会自动为你创建无参构造器。

## this关键字

**this** 关键字只能在非静态方法内部使用。当你调用一个对象的方法时，**this** 生成了一个对象引用。

### 在构造器中调用构造器

必须首先调用构造器而且只能调用一次

不允许在构造器之外的方法里调用构造器

### static的含义

static方法中不会存在this

## 垃圾回收器

1. 对象可能不被垃圾回收。
2. 垃圾回收不等同于析构

你创建的对象不是通过 **new** 来分配内存的，而垃圾回收器只知道如何释放用 **new** 创建的对象的内存，所以它不知道如何回收不是 **new** 分配的内存。为了处理这种情况，Java 允许在类中定义一个名为 `finalize()` 的方法。

### fi nalize()的用途

### 你必须实施清理

System.gc()

### 垃圾收集器如何工作

 C++/Java中的堆:

> Java: 传送带，传送带 + 重新排列
>
> C++: 院子，需要自己管理回收

垃圾回收机制:

> 1. 引用计数器
> 2. 对于任意"活"的对象，一定能最终追溯到其存活在栈或静态存储区中的引用。这个引用链条可能会穿过数个对象层次，由此，如果从栈或静态存储区出发，遍历所有的引用，你将会发现所有"活"的对象。
> 3. 在这种方式下，Java 虚拟机采用了一种*自适应*的垃圾回收技术。至于如何处理找到的存活对象，取决于不同的 Java 虚拟机实现。其中有一种做法叫做停止-复制（stop-and-copy）。
> 4. 要是没有新垃圾产生，就会转换到另一种模式（即"自适应"）。这种模式称为标记-清扫（mark-and-sweep）

"自适应的、分代的、停止-复制、标记-清扫"式的垃圾回收器

Java 虚拟机中有许多附加技术用来提升速度。尤其是与加载器操作有关的，被称为"即时"（Just-In-Time, JIT）编译器的技术。这种技术可以把程序全部或部分翻译成本地机器码，所以不需要 JVM 来进行翻译，因此运行得更快。

另一种做法称为*惰性评估*，意味着即时编译器只有在必要的时候才编译代码。

## 成员初始化

Java 尽量保证所有变量在使用前都能得到恰当的初始化。对于方法的局部变量，这种保证会以编译时错误的方式呈现，要是类的成员变量是基本类型，情况就会变得有些不同。正如在"万物皆对象"一章中所看到的，类的每个基本类型数据成员保证都会有一个初始值。

### 指定初始化

> 1. 在定义类成员变量的地方为其赋值
> 2. 通过调用某个方法来提供初值,这个方法可以带有参数，但这些参数不能是未初始化的类成员变量

## 构造器初始化

可以用构造器进行初始化，这种方式给了你更大的灵活性，因为你可以在运行时调用方法进行初始化。但是，这无法阻止自动初始化的进行，他会在构造器被调用之前发生。

### 初始化的顺序

在类中变量定义的顺序决定了它们初始化的顺序。即使变量定义散布在方法定义之间，它们仍会在任何方法（包括构造器）被调用之前得到初始化。

### 静态数据的初始化

无论创建多少个对象，静态数据都只占用一份存储区域。**static** 关键字不能应用于局部变量，所以只能作用于属性（字段、域）。如果一个字段是静态的基本类型，你没有初始化它，那么它就会获得基本类型的标准初值。如果它是对象引用，那么它的默认初值就是 **null**。

初始化的顺序先是静态对象（如果它们之前没有被初始化的话），然后是非静态对象

概括一下创建对象的过程，假设有个名为 **Dog** 的类：

1. 即使没有显式地使用 **static** 关键字，构造器实际上也是静态方法。所以，当首次创建 **Dog** 类型的对象或是首次访问 **Dog** 类的静态方法或属性时，Java 解释器必须在类路径中查找，以定位 **Dog.class**。
2. 当加载完 **Dog.class** 后（后面会学到，这将创建一个 **Class** 对象），有关静态初始化的所有动作都会执行。因此，静态初始化只会在首次加载 **Class** 对象时初始化一次。
3. 当用 `new Dog()` 创建对象时，首先会在堆上为 **Dog** 对象分配足够的存储空间。
4. 分配的存储空间首先会被清零，即会将 **Dog** 对象中的所有基本类型数据设置为默认值（数字会被置为 0，布尔型和字符型也相同），引用被置为 **null**。
5. 执行所有出现在字段定义处的初始化动作。
6. 执行构造器。你将会在"复用"这一章看到，这可能会牵涉到很多动作，尤其当涉及继承的时候。

### 显示静态初始化

可以将一组静态初始化动作放在类里面一个特殊的"静态子句"（有时叫做静态块）中

### 非静态实例初始化

Java 提供了被称为*实例初始化*的类似语法，用来初始化每个对象的非静态变量,看起来它很像静态代码块，只不过少了 **static** 关键字。这种语法对于支持"匿名内部类"（参见"内部类"一章）的初始化是必须的，但是你也可以使用它保证某些操作一定会发生，而不管哪个构造器被调用。从输出看出，实例初始化子句是在两个构造器之前执行的。

```java
class Mug {
    Mug(int marker) {
        System.out.println("Mug(" + marker + ")");
    }
}

public class Mugs {
    Mug mug1;
    Mug mug2;
    { // [1]
        mug1 = new Mug(1);
        mug2 = new Mug(2);
        System.out.println("mug1 & mug2 initialized");
    }

    Mugs() {
        System.out.println("Mugs()");
    }

    Mugs(int i) {
        System.out.println("Mugs(int)");
    }
}
```

## 数组初始化

数组是相同类型的、用一个标识符名称封装到一起的一个对象序列或基本类型数据序列。数组是通过方括号下标操作符 [] 来定义和使用的。要定义一个数组引用，只需要在类型名加上方括号：

```java
int[] a1;
```

方括号也可放在标识符的后面，两者的含义是一样的：

```java
int a1[];
```

```java
int[] a1 = {1, 2, 3, 4, 5}; // 初始化
```

### 动态数组创建

如果在编写程序时，不确定数组中需要多少个元素，那么该怎么办呢？你可以直接使用 **new** 在数组中创建元素。

### 可变参数列表

> java5之前采用数组进行可变参数
>
> Java5之后采用可变参数列表

```java
static void printArray(Object... args) {
        for (Object obj: args) {
            System.out.print(obj + " ");
        }
        System.out.println();
    }

```

## 枚举类型

enum

> 编译器还会创建 `ordinal()` 方法表示某个特定 **enum** 常量的声明顺序
>
> `static values()` 方法按照 enum 常量的声明顺序，生成这些常量值构成的数组

## 本章小节

构造器，这种看起来精巧的初始化机制，应该给了你很强的暗示：初始化在编程语言中的重要地位。