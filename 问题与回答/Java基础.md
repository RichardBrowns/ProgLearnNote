# Java概述

## Java有哪些优点？

- 跨平台特性：JVM在不同的操作系统中都有实现，只要在操作系统上装了JVM，就可以保证一个Java程序在不同的操作环境下都可以执行
- 自动内存管理：Java具有自动内存管理机制，避免手动管理，降低内存泄漏的风险，简化开发，使得程序更加健壮
- 标准库十分丰富：涵盖数据结构、I/O、多线程、网络编程等多个方面
- 强类型语言和异常处理机制：类型检查严格，异常处理完整，增强了代码的可靠性和安全性
- 生态强大和使用广泛：丰富的第三方框架，Spring家族，Apache Commons等等

## JVM、JRE、JDK有什么区别？

- JVM是Java的虚拟机，负责执行编译后的Java字节码
- JRE是Java的运行时环境，包含JVM和运行时需要的核心类库
- JDK包含JRE和javac、javadoc等开发工具，更加完整

## 什么是字节码，有什么用？

字节码，是.java文件经过编译后生成的.class文件，可以被虚拟机识别，并解释执行，从而实现Java的跨平台特性。

Java程序的执行过程主要有：

1. 编译：将代码(.java)编译成虚拟机可以识别理解的字节码(.class)
2. 解释：虚拟机解释字节码，生产操作系统可直接执行的机器码
3. 执行：机器执行二进制机器码

## JIT即时编译？

JIT即时编译器是存在是为了增强Java代码的执行效率，Java字节码解释的方式是不如C++编译成机器码执行的效率的。所以在一个Java程序执行开始，首先采用解释器模式执行Java字节码，在这过程中JIT编译器会实时监控Java程序的执行情况，识别出热点代码，并将其编译成本地机器码和进行各种优化，从而提升程序执行效率。

# 基础语法

## Java有哪些数据类型？

- 基本数据类型
  - boolean，默认false，大小因虚拟机实现而定
  - char，默认'\n0000'，2字节
  - byte，默认0，1字节
  - short，默认0，2字节
  - int，默认0，4字节
  - long，默认0L，8字节
  - float，默认0.0f，4字节
  - double，默认0.0，8字节
- 引用数据类型
  - 类，Class
  - 接口，interface
  - 数组，[]

## 基本数据类型-自动转换与强制转换？

**自动转换（隐式转换）**

规则：

- 范围较小的类型可以自动转换为范围较大的类型
- 没有数据丢失风险

转换顺序：

- byte -> short -> int -> long -> float -> double
- char -> int -> long -> float -> double

**强制转换（显式转换）**

规则：

- 需要显式地指定转换类型
- 可能造成数据丢失或精度降低

```
目标类型 变量名 = (目标类型) 值;
```

## 自动拆箱与装箱？

- **装箱：**将基本数据类型转换为对应的包装类型
- **拆箱：**将包装类型转换为基本数据类型

## 包装类的常量池？

常量池是JVM的一种优化机制，用于缓存常用的对象实例，避免对象频繁的创建和销毁，从而提升性能和节省内存。

**包装类的常量池**

- **Integer：**缓存-128~127的整型对象
- **Boolean：**缓存true和false实例
- **Character：**缓存\u0000~\u007F的字符对象
- **Byte：**缓存-128~127的所有字节对象
- **Short和Long：**也缓存-128~127的数值对象
- 浮点数没有对应的常量池

常量池的工作机制主要依赖于自动装箱（Autoboxing）和自动拆箱（Unboxing）。当你使用基本类型和包装类进行赋值或比较时，Java编译器会自动在基本类型和包装类之间进行转换。

## &和&&的区别？

- &是逻辑与，两边的表达式都会计算，全部为true时通过
- &&是短路与，先计算左边，如果左边是false，直接拒绝；同样两边都是true时通过

在做对象判空时要使用&&，比如`username != null &&!username.equals("")`，如果使用逻辑与，会造成空指针异常

***注意**：逻辑或（|）与短路或（||）的差别也是如此*

## switch语法支持哪些类型？

1. 整数类型：
   - byte
   - short
   - int
   - char
2. 枚举类型：
   - enum
3. 字符串类型：
   - String
4. 包装类型：
   - Byte
   - Short
   - Integer
   - Character

需要注意的是，`long`、`float`、`double`以及其他非整数类型（如`boolean`）不能用于`switch`语句。

## 什么是枚举类型？

Java中的枚举类型（`enum`）是一种特殊的数据类型，它允许一个变量只能是预定义的常量集合中的一个。用于表示一组固定的常量值，如一周中的七天、颜色、方向等。

枚举类型使用`enum`关键字定义。如：

```java
public enum Day {
    SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY
}
```

枚举不仅仅是简单的常量集合，它还可以包含构造函数、字段和方法。例如：

```java
public enum Day {
    SUNDAY("Weekend"), MONDAY("Weekday"), TUESDAY("Weekday"), 
    WEDNESDAY("Weekday"), THURSDAY("Weekday"), FRIDAY("Weekday"), 
    SATURDAY("Weekend");

    private String typeOfDay;

    private Day(String typeOfDay) {
        this.typeOfDay = typeOfDay;
    }

    public String getTypeOfDay() {
        return this.typeOfDay;
    }
}
```

枚举可以覆盖`toString()`方法来提供更友好的输出：

```java
public enum Day {
    SUNDAY("Weekend"), MONDAY("Weekday"), TUESDAY("Weekday"), 
    WEDNESDAY("Weekday"), THURSDAY("Weekday"), FRIDAY("Weekday"), 
    SATURDAY("Weekend");

    private String typeOfDay;

    private Day(String typeOfDay) {
        this.typeOfDay = typeOfDay;
    }

    public String getTypeOfDay() {
        return this.typeOfDay;
    }

    @Override
    public String toString() {
        return this.name() + " (" + this.typeOfDay + ")";
    }
}
```

枚举类型提供一些有用的静态方法：

- `values()`: 返回所有枚举常量的数组。
- `valueOf(String name)`: 根据名称返回对应的枚举常量。

## break，continue，return的用法和区别？

- break表示结束当前循环体
- continue表示结束本次循环，进行下一次循环
- return表示结束当前的方法，直接返回

## 什么是位运算？

位运算是直接对整数的二进制位进行操作的一种运算方式，效率较高。

- 左移运算符，<<：将一个二进制数的所有位向左移动若干位，右侧用0填充。左移一位相当于乘以2。
- 右移运算符，>>：将一个二进制数的所有位向右移动若干位，左侧用符号位填充。右移一位相当于除以2。
- 无符号右移运算符，>>>：将一个二进制数的所有位向右移动若干位，左侧用0填充，不考虑符号位。

## 自增/自减运算符？

在Java中，自增（`++`）和自减（`--`）运算符用于对变量进行增减操作。它们可以分别将变量的值增加或减少1。

- 前缀自增（`++variable`）和前缀自减（`--variable`）运算符首先对变量进行增减操作，然后再使用该变量的值。
- 后缀自增（`variable++`）和后缀自减（`variable--`）运算符首先使用变量的当前值，然后再对变量进行增减操作。

注意，这不是原子操作，所以如果在并发编程的环境下，要做进一步处理

## float怎么表示小数？

单精度浮点数使用 32 位（二进制位）来表示一个浮点数，这 32 位分为三个部分：

1. **符号位（Sign bit）**：1 位
2. **指数位（Exponent）**：8 位
3. **尾数位（Mantissa or Fraction）**：23 位

**1. 符号位（Sign bit）**

- 1 位，用于表示数的正负。
  - 0 表示正数
  - 1 表示负数

**2. 指数位（Exponent）**

- 8 位，用于表示指数部分。为了表示负指数，使用偏移量（bias）的方法。
  - 对于单精度浮点数，偏移量（bias）是 127。
  - 实际存储的指数值 = 指数 + 偏移量（127）

**3. 尾数位（Mantissa or Fraction）**

- 23 位，用于表示尾数（也称为有效数字）。
  - 尾数是一个二进制小数，隐含了一个前导的 1（即规范化形式）。
  - 实际存储的尾数是去掉这个前导的 1 的部分。

## 高精度浮点数？

有些业务对数值的精确度要求很高，所以可以使用BigDecimal类，这是因为 `BigDecimal` 提供了任意精度的十进制数表示，并且具有精确的舍入控制。

# 面向对象

## 面向过程与面向对象的区别？

- **面向过程**：分析出解决问题的步骤，使用函数实现一个个步骤，再从主函数开始进行链式调用
- **面向对象**：将遇到的问题构建为类和对象，描述其特征和行为，更接近日常思维，代码重用率更高。

## 封装、继承、多态？

- **封装：**封装是指将数据和行为的实现隐藏起来，限制外部对数据的直接访问，通过公共接口来操作对象的属性，从而提升代码的安全性和可维护性
- **继承：**继承允许一个类继承另一个类的属性和方法，从而实现代码复用和扩展。子类可以继承父类的功能，还可以增加新的功能或重写父类的方法。
- **多态：**多态性允许不同类的对象通过相同的接口调用，而不需要知道具体对象的类型。多态可以通过方法重载（同一类中方法名相同但参数不同）和方法重写（子类重写父类的方法）来实现，使得程序更加灵活和可扩展。

## 重载和重写的区别？

- **重载：**重载是指在同一个类，多个方法的名字相同，参数列表不同，目的是为了一个方法可以对不同的输入，做不同的输出
- **重写**：重写是指子类重写父类的方法，方法名、参数列表和返回值都一样，目的是为了在子类中提供特定的实现，以取代父类的实现。

## default、public、protected、private区别？

- default：表示同包可见
- private：同一类可见，不可修饰类和接口
- public：所有类可见
- protected：同包和子类可见，不可修饰类和接口

## this关键字？

this是一个引用，指向当前对象。所以可以使用它调用本对象的构造方法、属性和函数。

## 抽象类和接口的区别？

在Java中采用单继承机制，只可以继承一个类，但是可以实现多个接口。抽象类是is-a的语义，接口是has-a的语义。所以一个类要序列化，只要实现Serializable接口即可。抽象类更多的是为多个类提供共同的状态和行为。而接口则是定义了一组行为。

- 抽象类可以有自己的构造函数、变量、具体方法和抽象方法
- 接口没有自己的构造函数和变量，可以有常量，可以有方法签名和默认实现

## 成员变量和局部变量的区别？

- **成员变量**：成员变量属于对象的变量，至少在类内可见，生命周期与对象一致，有自己的默认值，和对象一道存在堆区，可以使用访问修饰符修饰
- **局部变量**：局部变量是方法、代码块的变量，作用域在方法和代码块以内，生命周期也是，存储在栈区

## 静态变量和实例变量？静态方法和实例方法？

- 静态变量，被static修饰，属于类，不同的对象共享这一个变量，在类加载时就存在
- 实例变量，依附于对象，通过对象进行访问
- 静态方法，也叫类方法，可以通过类名.方法名访问
- 实例方法，属于对象，通过对象名.方法名访问

## final关键字的作用？

- final作用于类时，表示该类不能被继承
- final作用于变量时，表示变量不能修改
- final作用于方法时，表示不能够重写

## == 和 equals()的区别？

- ==用于基本数据类型的比较，是比较变量的值
- ==用于引用数据类型的比较，是比较引用地址是否相等
- equals()方法一般表示两个对象的内容是否相等，属于Object的方法，默认和==相同，所以很多时候要按照自己的需要重写

## hashCode()和equals()?

**equals()方法**

`equals()`方法用于比较两个对象内容是否相等。默认情况下，`Object`类的`equals()`方法是比较对象的引用是否相同，即两个对象是否是同一个实例。然而，在实际应用中，我们通常需要比较对象的内容是否相等，因此需要重写`equals()`方法。

**hashCode()方法**

`hashCode()`方法返回对象的哈希码。哈希码是一个整数，用于支持基于哈希表的集合（如`HashMap`、`HashSet`等）。`Object`类的默认实现是基于对象的内存地址计算哈希码。

**equals()和hashCode()的关系**

- 如果两个对象根据`equals(Object)`方法是相等的，那么它们的`hashCode()`方法必须返回相同的整数。
- 如果两个对象根据`equals(Object)`方法是不相等的，它们的`hashCode()`方法可以返回相同的整数（但这样会降低哈希表的性能）。

## 参数传递机制？

Java采用值传递，不管是基础类型还是引用类型，传的都是值，方法拿到的是值的拷贝。

## 深拷贝和浅拷贝？

两者的区别主要在于当对象的成员变量是引用类型时，浅拷贝是拷贝的引用，所以两个对象共享一个成员对象；而深拷贝则会创建一个和之前一模一样的成员对象，再引用它。

## 创建对象的几种方式？

- new关键字创建

  ```java
  Person person = new Person();
  ```

- 反射机制创建

  ```java
  Class clazz = Class.forName("Person");
  Person person = (Person) clazz.newInstance();
  ```

- clone拷贝

  ```java
  Person person = new Person();
  Person person2 = (Person) person.clone();
  ```

- 序列化机制创建

  ```java
  Person person = new Person();
  ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.txt"));
  oos.writeObject(person);
  ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.txt"));
  Person person2 = (Person) ois.readObject();
  ```

## 子父类静态代码块和构造方法执行顺序？

1. **父类的静态代码块**（只执行一次，在类加载时执行）
2. **子类的静态代码块**（只执行一次，在类加载时执行）
3. **父类的实例代码块**（每次创建对象时执行）
4. **父类的构造方法**（每次创建对象时执行）
5. **子类的实例代码块**（每次创建对象时执行）
6. **子类的构造方法**（每次创建对象时执行）

# String

## String的基本特性？

`String` 类是 Java 中用于表示字符串的类。

- 一旦创建了一个 `String` 对象，它的值就不能被改变。任何对 `String` 对象的修改都会创建一个新的 `String` 对象，而不会改变原有的对象。
- 为了提高效率和节省内存，Java 使用了字符串池。当你创建一个字符串字面量时，Java 会先检查池中是否已经存在相同的字符串。如果存在，则直接返回池中的引用；如果不存在，则创建一个新的字符串并放入池中。
- 常用方法：
  - `length()`: 返回字符串的长度。
  - `charAt(int index)`: 返回指定索引处的字符。
  - `substring(int beginIndex, int endIndex)`: 返回子字符串。
  - `indexOf(String str)`: 返回子字符串第一次出现的索引。
  - `toUpperCase()`: 转换为大写。
  - `toLowerCase()`: 转换为小写。
  - `trim()`: 去除字符串两端的空白字符。
  - `equals(Object anObject)`: 比较字符串内容是否相同。
  - `equalsIgnoreCase(String anotherString)`: 忽略大小写比较字符串内容是否相同。
  - `split(String regex)`: 根据正则表达式分割字符串。

## String、StringBuffer和StringBuilder的区别？

在 Java 中，`String`、`StringBuilder` 和 `StringBuffer` 都是用于处理字符串的类，它们有以下区别：

1. 可变性
   - String对象不可变
   - StringBuilder和StringBuffer可变
2. 线程安全
   - String由于不可变，所以线程安全
   - StringBuilder线程不安全，StringBuffer线程安全
3. 性能
   - String修改的性能差
   - StringBuilder单线程环境性能好
   - StringBuffer性能略弱于StringBuilder，但是线程安全

## String str1 = new String("abc") 和 String str2 = "abc" 的区别？

- 直接使用双引号为字符串变量赋值时，首先会检查字符串常量池里是否存在相同的字符串，如果存在，就返回这个字符串的引用。如果不存在，就创建一个新的字符串，并把它放入常量池，并返回引用
- 使用new关键字创建，每次都会重新创建新的字符串，且不会放入常量池

## intern方法有什么作用？

在 Java 中，`intern()` 方法是 `String` 类的一个实例方法，它的作用是将字符串放入字符串池（String Pool）中，并返回字符串池中的字符串对象引用。如果字符串池中已经包含了一个与当前字符串内容相同的字符串对象，则直接返回池中的字符串对象引用；如果字符串池中没有包含该字符串，则将当前字符串添加到字符串池中，并返回其引用。

# Object

## Object有哪些方法？

1. **对象比较**
   - **`public boolean equals(Object obj)`**:
     - 用于比较两个对象是否相等。默认实现是比较对象的引用是否相同。可以在子类中重写该方法以实现自定义的比较逻辑。
   - **`public int hashCode()`**:
     - 返回对象的哈希码值。哈希码用于基于哈希的集合类（如 `HashMap`、`HashSet`）。可以在子类中重写该方法以确保与 `equals` 方法一致。
2. **对象拷贝**
   - **`protected Object clone()`**:
     - 创建并返回当前对象的一个副本。要使用这个方法，类必须实现 `Cloneable` 接口，并重写 `clone` 方法。默认浅拷贝。
3. **对象转字符串**
   - **`public String toString()`**:
     - 返回对象的字符串表示形式。默认实现返回对象的类名和哈希码。可以在子类中重写该方法以提供更有意义的字符串表示。
4. **垃圾回收**
   - **`protected void finalize()`**:
     - 当垃圾收集器确定不再有对该对象的引用时，调用该方法。可以在子类中重写该方法以清理资源。
5. **反射**
   - **`public final Class<?> getClass()`**:
     - 返回对象的运行时类。
6. **多线程调度**
   - **`public final void wait()`**:
     - 导致当前线程等待，直到其他线程调用此对象的 `notify()` 方法或 `notifyAll()` 方法。
   - **`public final void wait(long timeout)`**:
     - 导致当前线程等待，直到其他线程调用此对象的 `notify()` 方法或 `notifyAll()` 方法，或者指定的时间已过。
   - **`public final void wait(long timeout, int nanos)`**:
     - 导致当前线程等待，直到其他线程调用此对象的 `notify()` 方法或 `notifyAll()` 方法，或者指定的时间（以毫秒和纳秒为单位）已过。
   - **`public final void notify()`**:
     - 唤醒在此对象监视器上等待的单个线程。
   - **`public final void notifyAll()`**:
     - 唤醒在此对象监视器上等待的所有线程。

# 异常处理

## 异常处理的层次结构？

<img src="https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/javase-22.png" alt="三分恶面渣逆袭：Java异常体系" style="zoom:67%;" />

1. **Throwable**

- 所有错误和异常的超类。

2. **Error**

- 表示严重的系统错误，程序一般不应该捕获这些错误。
- 常见子类：
  - `OutOfMemoryError`
  - `StackOverflowError`
  - `VirtualMachineError`

3. **Exception**

- 表示程序本身可以捕获和处理的异常。

- 3.1 **运行时异常（RuntimeException）**

  - 在编译时不强制要求捕获或声明。

  - 常见子类：
    - `NullPointerException`
    - `ArrayIndexOutOfBoundsException`
    - `ArithmeticException`
    - `ClassCastException`
    - `IllegalArgumentException`

- 3.2 **非运行时异常（Checked Exception）**

  - 在编译时必须捕获或声明的异常。

  - 常见子类：
    - `IOException`
      - `FileNotFoundException`
      - `EOFException`
    - `SQLException`
    - `ClassNotFoundException`
    - `InterruptedException`

## 异常的处理方式？

1. try-catch块
   - `try-catch`块用于捕获和处理可能在`try`块中发生的异常。`catch`块用于处理特定类型的异常。
   - **`try`块**：包含可能抛出异常的代码。
   - **`catch`块**：用于捕获并处理特定类型的异常。可以有多个`catch`块，处理不同类型的异常。
   - **`finally`块**：无论是否发生异常，`finally`块中的代码都会执行。通常用于清理资源，例如关闭文件或数据库连接。
2. throws关键字
   - `throws`关键字用于在方法声明中指定该方法可能抛出的异常类型。调用该方法的代码必须处理这些异常，或者继续声明抛出。
3. throw关键字
   - `throw`关键字用于显式地抛出一个异常对象。
4. 自定义异常
   - 有时内置的异常类不能完全表达某种错误情况，这时可以通过继承`Exception`或`RuntimeException`类来创建自定义异常。
5. 多重捕获
   - 可以在一个`catch`块中捕获多个异常类型。
6. try-with-resource
   - Java 7 引入的`try-with-resources`语句用于自动管理资源。任何实现了`AutoCloseable`接口的资源都可以在`try`块中自动关闭。

# I/O

## I/O流分为哪几种？

在Java中，I/O流（Input/Output Stream）用于处理输入和输出操作。

1. 按数据流的方向分类
   - 输入流：用于从数据源读取数据到程序。
   - 输出流：用于程序向数据目标写入数据
2. 按数据处理方式分类
   - 字节流：用于处理字节数据，适合于所有类型的I/O操作，尤其是二进制数据
   - 字符流：用于处理字符数据，适用于文本文件的I/O操作
3. 按数据流的功能分类
   - 输入字节流：
     - `InputStream`：字节输入流的抽象基类。
     - `FileInputStream`：用于从文件中读取字节。
     - `ByteArrayInputStream`：用于从字节数组中读取字节。
     - `FilterInputStream`：所有过滤字节输入流的父类。
     - `BufferedInputStream`：为另一个输入流添加一些功能，即缓冲输入流。
     - `DataInputStream`：允许应用程序以机器无关的方式从底层输入流中读取基本 Java 数据类型。
     - `ObjectInputStream`：用于从流中反序列化对象。
   - 输出字节流：
     - `OutputStream`：字节输出流的抽象基类。
     - `FileOutputStream`：用于将字节写入文件。
     - `ByteArrayOutputStream`：用于将字节写入字节数组。
     - `FilterOutputStream`：所有过滤字节输出流的父类。
     - `BufferedOutputStream`：为另一个输出流添加一些功能，即缓冲输出流。
     - `DataOutputStream`：允许应用程序以机器无关的方式将基本 Java 数据类型写入输出流。
     - `ObjectOutputStream`：用于将对象序列化到流中。
   - 输入字符流：
     - `Reader`：字符输入流的抽象基类。
     - `FileReader`：用于从文件中读取字符。
     - `CharArrayReader`：用于从字符数组中读取字符。
     - `BufferedReader`：为另一个输入字符流添加一些功能，即缓冲输入字符流。
     - `InputStreamReader`：将字节流转换为字符流。
     - `StringReader`：用于从字符串中读取字符。
   - 输出字符流：
     - `Writer`：字符输出流的抽象基类。
     - `FileWriter`：用于将字符写入文件。
     - `CharArrayWriter`：用于将字符写入字符数组。
     - `BufferedWriter`：为另一个输出字符流添加一些功能，即缓冲输出字符流。
     - `OutputStreamWriter`：将字符流转换为字节流。
     - `StringWriter`：用于将字符写入字符串。
4. 按功能细分的其他流
   - 数据流：用于读写原始数据类型。
     - `DataInputStream` 和 `DataOutputStream`。
   - 对象流：用于读写对象。
     - `ObjectInputStream` 和 `ObjectOutputStream`。
   - 缓冲流：用于提高I/O操作的效率。
     - `BufferedInputStream`，`BufferedOutputStream`，`BufferedReader`，`BufferedWriter`。
   - 转换流：用于字节流和字符流之间的转换。
     - `InputStreamReader` 和 `OutputStreamWriter`。

## 有了字节流，为什么还要字符流？

- 字符流可以处理字符编码问题，避免乱码
- 有更高层次抽象的封装，比如可以直接读取整行文本
- 比使用字节流处理效率更高

## 说一下BIO、NIO、AIO和它们之间的区别？

Java提供了三种主要的I/O模型：BIO（Blocking I/O）、NIO（Non-blocking I/O）和AIO（Asynchronous I/O）。

1. **BIO（Blocking I/O）**

   - **特点**
     - **阻塞模式**：BIO是传统的I/O模型，I/O操作是阻塞的。也就是说，当一个线程进行I/O操作时，如果没有数据可读或无法写入数据，线程会被阻塞，直到数据准备好。
     - **一个连接一个线程**：在BIO模型中，每个客户端连接都会占用一个独立的线程，这在高并发场景下会导致大量的线程创建和销毁，增加系统的开销。
   - **适用场景**
     - 适用于连接数较少且固定的场景。
     - 适用于对实时性要求不高的应用。

2. **NIO（Non-blocking I/O）**

   - **特点**
     - **非阻塞模式**：NIO引入了非阻塞I/O操作，线程可以在等待I/O操作完成的同时执行其他任务。
     - **单线程处理多连接**：NIO使用了选择器（Selector）机制，一个线程可以管理多个客户端连接，通过轮询的方式检查I/O事件。
     - **缓冲区**：NIO引入了缓冲区（Buffer）来读写数据，数据先存储在缓冲区中，然后再进行处理。
   - **适用场景**
     - 适用于连接数较多且连接时间较长的场景。
     - 适用于对实时性要求较高的应用。

3. **AIO（Asynchronous I/O）**

   - **特点**
     - **异步非阻塞模式**：AIO是异步非阻塞I/O模型，I/O操作是异步的，操作完成后会通过回调机制通知应用程序。
     - **简化编程**：AIO简化了编程模型，因为I/O操作是异步的，线程不需要等待I/O操作完成。
   - **适用场景**
     - 适用于连接数较多且连接时间较长的场景。
     - 适用于对实时性要求较高的应用。
     - 适用于复杂的I/O操作，如文件传输等。

4. **区别总结**

   1. **阻塞与非阻塞**：
      - BIO：阻塞I/O，线程会被阻塞，直到I/O操作完成。
      - NIO：非阻塞I/O，线程可以在等待I/O操作完成的同时执行其他任务。
      - AIO：异步非阻塞I/O，I/O操作是异步的，操作完成后通过回调机制通知应用程序。
   2. **线程模型**：
      - BIO：一个连接一个线程。
      - NIO：一个线程可以管理多个连接，通过选择器机制进行管理。
      - AIO：一个线程可以管理多个连接，通过异步回调机制进行管理。
   3. **适用场景**：
      - BIO：适用于连接数较少且固定的场景，对实时性要求不高的应用。
      - NIO：适用于连接数较多且连接时间较长的场景，对实时性要求较高的应用。
      - AIO：适用于连接数较多且连接时间较长的场景，对实时性要求较高的应用，特别适用于复杂的I/O操作。

5. **总结**

   BIO、NIO和AIO各有优缺点和适用场景。选择哪种I/O模型取决于具体的应用需求和场景。在高并发和高实时性要求的应用中，NIO和AIO通常是更好的选择，而在简单的、连接数较少的应用中，BIO可能更为适用。

# 序列化

## 什么是序列化？什么是反序列化？

Java序列化是一种机制，通过它可以将对象的状态转换为字节流，以便将对象保存到文件、数据库，或者通过网络传输到另一个Java虚拟机（JVM）。反序列化则是将字节流恢复为对象的过程。

**序列化的前提**

1. **实现Serializable接口**
   - 一个类必须实现`java.io.Serializable`接口才能使其对象可序列化。这个接口是一个标记接口（没有任何方法），它只是告诉JVM这个类的对象可以被序列化。
2. **serialVersionUID**
   - `serialVersionUID`是一个唯一的标识符，用于版本控制。如果类的定义发生变化（如添加或删除字段），会影响序列化和反序列化的兼容性。显式声明`serialVersionUID`可以避免因类的修改导致的反序列化失败。

- `serialVersionUID`是一个唯一的标识符，用于版本控制。如果类的定义发生变化（如添加或删除字段），会影响序列化和反序列化的兼容性。显式声明`serialVersionUID`可以避免因类的修改导致的反序列化失败。

**序列化的控制**

1. **transient关键字**
   - 使用`transient`关键字修饰的字段不会被序列化。
2. **自定义序列化**
   - 通过实现`readObject`和`writeObject`方法，可以自定义序列化和反序列化过程。

**序列化的应用场景**

1. **持久化对象状态**：将对象的状态保存到文件或数据库中，以便在以后恢复。
2. **网络传输**：通过网络传输对象，例如在分布式系统中，多个JVM之间传递对象。
3. **缓存**：将对象序列化后存储在缓存中，以便快速恢复对象状态。

**序列化的注意事项**

1. **版本控制**：确保`serialVersionUID`一致，以避免版本不兼容问题。
2. **安全性**：序列化和反序列化过程可能会引入安全漏洞，特别是反序列化时。如果字节流来自不可信的来源，可能会导致反序列化漏洞。因此，应该避免反序列化不可信的数据。
3. **性能**：序列化和反序列化是一个相对昂贵的操作，可能会影响性能。在高性能要求的场景下，需要谨慎使用。

## 序列化的几种方式？

- **Java原生序列化**：这是最常见和基础的序列化方式，通过实现`java.io.Serializable`接口来实现对象的序列化和反序列化。
- **Json序列化**：使用第三方库（如Jackson、Gson）将对象序列化为JSON格式。JSON格式具有可读性强、跨语言支持好等优点。
- **XML序列化**：使用第三方库（如XStream、JAXB）将对象序列化为XML格式。
- **ProtoBuff序列化**：Protocol Buffers是Google开发的一种高效的二进制序列化格式，适用于跨语言的数据交换。

# 泛型

## 什么是Java泛型？常见的通配符？

Java泛型是Java 5引入的一种语言特性，允许在定义类、接口和方法时使用类型参数，从而使代码更加通用和类型安全。泛型的主要目的是在编译时提供类型检查，并减少类型转换的需要。

**基本概念**

1. **泛型类**：在类定义中使用类型参数。
2. **泛型接口**：在接口定义中使用类型参数。
3. **泛型方法**：在方法定义中使用类型参数。

**常见的通配符**

Java泛型中的通配符用于表示未知类型，主要有以下几种：

1. **无界通配符**（`<?>`）
   - 无界通配符表示任何类型。它通常用于表示对类型参数没有任何限制的情况。
2. **上界通配符**（`<? extends T>`）
   - 上界通配符表示类型参数必须是指定类型的子类型（包括指定类型本身）。
3. **下界通配符**（`<? super T>`）
   - 下界通配符表示类型参数必须是指定类型的超类型（包括指定类型本身）。

# 注解

## 什么是注解？

注解（Annotation）是Java中的一种元数据（metadata）机制，允许在代码中添加额外的信息。注解可以用于类、方法、字段、参数、局部变量等各种元素，并且这些信息可以在编译时或运行时通过反射机制进行访问和处理。

**基本概念**

1. **元注解（Meta-Annotation）**：用于定义其他注解的注解。
2. **内置注解**：Java提供的一些常用注解。
3. **自定义注解**：用户可以根据需要定义自己的注解。

**常见的元注解**

1. **@Retention**：指定注解的保留策略。
   - **RetentionPolicy.SOURCE**：注解只在源代码中存在，编译后会被丢弃。
   - **RetentionPolicy.CLASS**：注解在编译时存在于类文件中，但在运行时不可见（默认策略）。
   - **RetentionPolicy.RUNTIME**：注解在运行时可通过反射机制访问。
2. **@Target**：指定注解可以应用的程序元素。
   - **ElementType.TYPE**：类、接口、枚举。
   - **ElementType.FIELD**：字段。
   - **ElementType.METHOD**：方法。
   - **ElementType.PARAMETER**：参数。
   - **ElementType.CONSTRUCTOR**：构造方法。
   - **ElementType.LOCAL_VARIABLE**：局部变量。
   - **ElementType.ANNOTATION_TYPE**：注解类型。
   - **ElementType.PACKAGE**：包。
3. **@Documented**：指定注解是否包含在Javadoc中。
4. **@Inherited**：指定注解是否可以被子类继承。

**常见的内置注解**

1. **@Override**：表示方法是重写父类方法。
2. **@Deprecated**：表示方法、类或字段已过时，不建议使用。
3. **@SuppressWarnings**：表示抑制编译器警告。

**自定义注解**

用户可以根据需要定义自己的注解。

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface MyAnnotation {
    String value();
}

public class MyClass {
    @MyAnnotation("Hello")
    public void myMethod() {
        System.out.println("My Method");
    }
}
```

**反射机制获取注解信息**

通过反射机制可以在运行时获取注解信息。

```java
import java.lang.reflect.Method;

public class Main {
    public static void main(String[] args) throws Exception {
        Method method = MyClass.class.getMethod("myMethod");
        MyAnnotation annotation = method.getAnnotation(MyAnnotation.class);
        System.out.println(annotation.value());
    }
}
```

**总结**

1. **注解**：用于在代码中添加元数据，可以应用于类、方法、字段等。
2. **元注解**：用于定义注解的注解，如`@Retention`、`@Target`等。
3. **内置注解**：Java提供的常用注解，如`@Override`、`@Deprecated`等。
4. **自定义注解**：用户可以根据需要定义自己的注解。
5. **反射机制**：可以在运行时获取注解信息。

# 反射

## 什么是反射？有什么应用？原理是什么？

反射（Reflection）是Java语言中的一种机制，允许程序在运行时检查和修改自身的结构和行为。通过反射，程序可以动态地获取类的相关信息（如类名、方法、字段、构造方法等），并且可以在运行时创建对象、调用方法和访问字段。

**反射的基本概念**

1. **Class对象**：每个类在运行时都有一个`Class`对象，包含了关于该类的所有信息。
2. **Method对象**：表示类的方法，可以用来调用方法。
3. **Field对象**：表示类的字段，可以用来访问和修改字段的值。
4. **Constructor对象**：表示类的构造方法，可以用来创建新的实例。

**反射的应用场景**

1. **框架和库**：如Spring、Hibernate等框架广泛使用反射来实现依赖注入、AOP（面向切面编程）等功能。
2. **动态代理**：Java的动态代理机制利用反射来创建代理对象。
3. **工具和IDE**：如Eclipse、IntelliJ IDEA等开发工具使用反射来提供代码分析和自动补全功能。
4. **序列化和反序列化**：如JSON、XML解析库使用反射来将对象转换为数据格式，或从数据格式还原对象。
5. **测试框架**：如JUnit使用反射来调用测试方法。

**反射的原理**

反射的核心是`java.lang.reflect`包，该包提供了一些类和接口，用于获取类的结构信息和操作类的成员。

1. **获取Class对象**：可以通过以下几种方式获取类的`Class`对象：
   - 使用`Class.forName("类的全限定名")`。
   - 使用`类名.class`。
   - 使用`对象.getClass()`。
2. **获取类的信息**：通过`Class`对象可以获取类的构造方法、字段、方法等信息。
   - `getConstructors()`：获取所有公共构造方法。
   - `getDeclaredConstructors()`：获取所有构造方法（包括私有、保护、默认、公有）。
   - `getMethods()`：获取所有公共方法，包括从父类继承的方法。
   - `getDeclaredMethods()`：获取所有方法（包括私有、保护、默认、公有）。
   - `getFields()`：获取所有公共字段。
   - `getDeclaredFields()`：获取所有字段（包括私有、保护、默认、公有）。
3. **创建对象**：通过反射可以动态地创建类的实例。
   - 使用`newInstance()`方法。
   - 使用`Constructor`对象的`newInstance()`方法。
4. **调用方法**：通过反射可以动态地调用类的方法。
   - 使用`Method`对象的`invoke()`方法。
5. **访问字段**：通过反射可以动态地访问和修改类的字段。
   - 使用`Field`对象的`get()`和`set()`方法。

**反射的优缺点**

**优点**：

1. **动态性**：反射允许在运行时动态地操作类和对象，增强了程序的灵活性和可扩展性。
2. **通用性**：许多框架和库使用反射来实现通用的功能，如依赖注入、序列化等。

**缺点**：

1. **性能开销**：反射操作通常比直接调用慢，因为需要进行动态解析。
2. **安全性问题**：反射可以绕过访问控制，可能会导致安全漏洞。
3. **复杂性**：反射代码通常较为复杂，不易理解和维护。

# JDK1.8 特性

## Lambda表达式？

Lambda表达式引入了一种更简洁的方式来表示匿名函数，使代码更简洁和易读。Lambda表达式的语法如下：

```java
(parameters) -> expression
或
(parameters) -> { statements; }
```

示例：

```java
// 传统方式
new Thread(new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello, world!");
    }
}).start();

// Lambda表达式
new Thread(() -> System.out.println("Hello, world!")).start();
```

## 函数式接口？

函数式接口是只包含一个抽象方法的接口，可以通过Lambda表达式来实例化。JDK 8引入了`@FunctionalInterface`注解来标识函数式接口。常见的函数式接口有`Runnable`、`Callable`、`Comparator`等。

```java
@FunctionalInterface
public interface MyFunctionalInterface {
    void myMethod();
}

// 使用Lambda表达式
MyFunctionalInterface myFunc = () -> System.out.println("Hello Functional Interface");
myFunc.myMethod();
```

## 方法引用和构造器引用？

方法引用和构造器引用提供了一种简洁的方式来引用现有的方法或构造器。

```java
// 方法引用
Consumer<String> print = System.out::println;
print.accept("Hello Method Reference");

// 构造器引用
Supplier<List<String>> listSupplier = ArrayList::new;
List<String> list = listSupplier.get();
```

## 默认方法和静态方法？

JDK 8允许在接口中定义默认方法和静态方法。默认方法使用`default`关键字定义，可以有方法体；静态方法使用`static`关键字定义。

```java
public interface MyInterface {
    default void defaultMethod() {
        System.out.println("Default Method");
    }

    static void staticMethod() {
        System.out.println("Static Method");
    }
}

public class MyClass implements MyInterface {
    public static void main(String[] args) {
        MyClass myClass = new MyClass();
        myClass.defaultMethod();
        MyInterface.staticMethod();
    }
}
```

## Stream API？

Stream API提供了一种高效且易于使用的方式来处理集合数据。它支持各种操作，如过滤、映射、归约等。

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
names.stream()
     .filter(name -> name.startsWith("A"))
     .forEach(System.out::println);
```

## Optional类

`Optional`类用于防止`NullPointerException`，提供了一种优雅的方式来处理可能为null的值。

## 新的日期和时间API

JDK 8引入了全新的日期和时间API（`java.time`包），提供了更好的日期和时间处理功能，如`LocalDate`、`LocalTime`、`LocalDateTime`、`ZonedDateTime`等。

## Base64编码和解码

JDK 8提供了内置的Base64编码和解码功能。