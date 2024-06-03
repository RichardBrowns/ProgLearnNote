# Java基础

## 面向对象

面向对象编程（Object-Oriented Programming, OOP）是一种编程范式，其核心思想包括封装、继承、多态三大特性。

### 封装

封装（Encapsulation）是基础也是最重要的概念之一，它主要体现在以下几个方面：

1. 数据隐藏
   - 定义：封装意味着将对象的状态（数据成员，即属性）和行为（成员方法，即函数）捆绑在一起，并对外界隐藏其内部实现细节。这样做的目的是减少外部对内部数据的直接访问，从而保护数据的完整性和安全性
   - 实现方式：通过使用访问修饰符（如Java中的private, protected, C++中的private, protected）来限制对类成员的直接访问。只暴露有限的公共接口（public方法）供外部调用。
2. 模块化
   - 概念：封装鼓励将复杂系统分解为一系列相互独立的模块（类），每个模块负责一个特定的功能。这种模块化设计使得软件更易于理解、维护和重用。
   - 好处：提高了代码的可维护性和可扩展性。当需要修改或扩展功能时，只需调整相关的类而不会影响到其他部分，降低了各部分间的耦合度。
3. 提高复用性
   - 说明：封装好的类可以作为组件在不同的程序中重复使用，无需关心其内部实现细节。这大大提高了代码的复用率，减少了重复编写相似代码的工作量。
4. 设计灵活性
   - 体现：封装允许在不改变接口的情况下修改类的内部实现，这称为“开闭原则”（Open for Extension, Closed for Modification）。这意味着可以在不影响现有客户端代码的情况下增加新功能或优化性能。

**实例说明**

```java
public class BankAccount {
    // 封装的数据（私有成员变量）
    private String accountNumber;
    private double balance;

    // 构造方法，用于初始化账户
    public BankAccount(String accountNumber, double initialBalance) {
        this.accountNumber = accountNumber;
        this.balance = initialBalance;
    }

    // 公共接口，提供对外操作余额的方法
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
        }
    }

    public double getBalance() {
        return balance;
    }
}

```

在这个例子中，accountNumber和balance被声明为private，防止外部直接修改这些数据，确保了数据的安全性。而通过deposit、withdraw和getBalance这些公共方法，外界可以安全地与BankAccount对象交互，实现了对内部状态的操作，体现了封装的思想。

### 继承

Java的继承（Inheritance）是一种创建新类的方式，新类（子类、派生类）继承了现有类（父类、基类）的特性和行为，并可以在此基础上添加新的特性或重写父类的方法。继承是面向对象的四大基本原则（封装、继承、多态、抽象）之一，其核心目的和特点包括：

1. 代码复用
   - 继承允许子类直接获得父类的属性和方法，无需重复编写相同的代码。这大大提高了代码的可复用性，减少了冗余。
2. 层次结构
   - 继承支持构建类的层次结构，形成“is-a”关系。例如，一个Bird类可以从Animal类继承，表示鸟是一种动物。这种层次结构有助于组织和管理类，使其逻辑更加清晰。
3. 多态性增强
   - 继承是实现多态的基础。子类可以覆盖或重写父类的方法，同时保留父类接口的一致性。这意味着，针对父类类型的引用可以指向子类的对象，并能调用子类特有的实现，增强了程序的灵活性和扩展性。
4. 特性扩展与修改
   - 子类可以在继承父类的基础上，添加新的属性和方法，或者重写父类的方法来改变其行为。这使得子类能够拥有比父类更丰富的功能，同时也便于维护和升级。

**实现方式**

在大多数面向对象的编程语言中，继承的语法通常涉及关键字如extends（Java, C#）、:(Python) 或 inherits(VB.NET) 等。以下是一个简单的示例，展示了继承的基本用法：

```java
// 父类（基类）
public class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    public void eat() {
        System.out.println(name + " is eating.");
    }
}

// 子类（派生类）
public class Dog extends Animal {
    public Dog(String name) {
        super(name); // 调用父类构造方法
    }
    
    // 重写父类方法
    @Override
    public void eat() {
        System.out.println(name + " is eating dog food.");
    }
    
    // 新增方法
    public void bark() {
        System.out.println(name + " is barking.");
    }
}

// 使用示例
public class Main {
    public static void main(String[] args) {
        Dog myDog = new Dog("Buddy");
        myDog.eat(); // 输出: Buddy is eating dog food.
        myDog.bark(); // 输出: Buddy is barking.
    }
}
```

在这个例子中，Dog类继承了Animal类，获得了eat方法，并且重写了该方法以适应狗的行为。此外，Dog类还新增了一个bark方法，展示了如何在继承的基础上扩展功能。

### 多态

面向对象编程中的多态（Polymorphism）是指允许不同类的对象对同一消息做出响应的能力，即同一个接口可以被不同的对象以不同的方式实现。多态是面向对象编程的四大基本原则之一，其核心在于“一个接口，多种实现”，旨在提高代码的灵活性和可扩展性。多态主要通过以下几种方式体现：

1. 方法重写（Overriding）
   - 当子类继承自父类时，子类可以重写（Override）父类中的某些方法，提供自己的实现。尽管方法名和参数列表相同，但执行的行为可能不同，这就是多态的一种体现。
2. 接口实现（Interface Implement）
   - 接口定义了一组方法的协议，任何实现了该接口的类都需要提供这些方法的具体实现。不同的类可以以各自的方式实现相同的接口，从而表现出多态性。
3. 抽象类与抽象方法
   - 抽象类不能实例化，它包含抽象方法（没有具体实现的方法）。子类继承抽象类时，必须实现所有的抽象方法，这同样支持了多态的概念。
4. 对象的向上转型（Upcasting）
   - 在Java等语言中，一个子类对象可以被赋值给一个父类引用，这个过程称为向上转型。此时，通过父类引用调用被子类重写的方法时，会执行子类的实现，这也是多态的直接表现。
5. 动态绑定（Dynamic Binding / Late Binding）
   - 动态绑定是指在运行时决定调用哪个方法实现的过程。这意味着，即使是通过父类引用调用方法，实际执行的是子类中重写的方法，这一决策过程是在程序运行时完成的，而非编译时。

**实例说明**

```java
// 定义一个动物接口
interface Animal {
    void makeSound();
}

// 狗类实现动物接口
class Dog implements Animal {
    public void makeSound() {
        System.out.println("Dog barks.");
    }
}

// 猫类实现动物接口
class Cat implements Animal {
    public void makeSound() {
        System.out.println("Cat meows.");
    }
}

// 主函数演示多态
public class PolymorphismDemo {
    public static void main(String[] args) {
        Animal myAnimal; // 声明一个动物类型的引用

        myAnimal = new Dog(); // 实例化一个狗对象
        myAnimal.makeSound(); // 输出: Dog barks.

        myAnimal = new Cat(); // 同样的引用现在指向猫对象
        myAnimal.makeSound(); // 输出: Cat meows.
    }
}

```

在这个例子中，Animal接口定义了一个makeSound方法，而Dog和Cat类分别实现了这个接口，并提供了不同的实现。通过将不同类型的对象赋值给Animal类型的引用，我们能够在运行时根据实际对象类型动态调用相应的方法，展示了多态的特性。

### 抽象

面向对象编程中的抽象（Abstraction）是一种将现实世界中的实体转化为计算机模型的过程，它关注的是对象的主要特征和行为，忽略不必要的细节。抽象在面向对象编程中主要体现在两个层面：

1. 抽象类（Abstract Class）
   - 抽象类是不能被实例化的类，它通常包含抽象方法（没有具体实现的方法）和非抽象方法。抽象类用于定义一个类族的共同特征，它的子类必须实现所有的抽象方法，以提供具体实现。
   - 抽象类允许定义一些通用的行为，子类可以继承这些行为并根据需要进行扩展。这有助于保持代码的整洁和模块化，同时降低了类之间的耦合。
2. 接口（Interface）
   - 接口是另一种形式的抽象，它定义了一组方法签名，但不提供实现。一个类可以实现一个或多个接口，以表明它支持这些接口所定义的行为。接口主要用于实现多继承，因为Java等语言不支持多重继承（一个类只能继承一个父类）。
   - 接口强制实现者提供所有方法的实现，确保了接口定义的行为在所有实现者中保持一致。

**抽象的好处**

1. 信息隐藏：抽象帮助隐藏实现细节，使代码更易于理解和维护。
2. 模块化：通过抽象，可以将复杂系统分解为相互独立的模块，每个模块专注于其核心功能。
3. 代码重用：抽象类和接口可以作为其他类的基础，提高代码复用性。
4. 灵活性和可扩展性：抽象类和接口允许在不修改现有代码的情况下添加新功能，符合开闭原则。

**实例说明**

```java
// 抽象类Vehicle
abstract class Vehicle {
    abstract void start();
    void move() {
        System.out.println("Vehicle is moving.");
    }
}

// 接口Engine
interface Engine {
    void accelerate();
    void brake();
}

// 子类Car继承抽象类Vehicle并实现Engine接口
class Car extends Vehicle implements Engine {
    public void start() {
        System.out.println("Car engine started.");
    }

    public void accelerate() {
        System.out.println("Car is accelerating.");
    }

    public void brake() {
        System.out.println("Car is braking.");
    }
}

// 主函数
public class AbstractionDemo {
    public static void main(String[] args) {
        Car myCar = new Car();
        myCar.start();
        myCar.move();
        myCar.accelerate();
        myCar.brake();
    }
}

```

在这个例子中，Vehicle是一个抽象类，包含一个抽象方法start()和一个非抽象方法move()。Engine是一个接口，定义了accelerate()和brake()方法。Car类继承了Vehicle并实现了Engine接口，提供了所有抽象方法的实现。通过这种方式，Car类继承了Vehicle的抽象行为，并实现了Engine接口所定义的行为。

## 知识点

### 数据类型

在Java编程语言中，数据类型分为两大类：基本数据类型（Primitive Data Types）和引用数据类型（Reference Data Types）。

1. 基本数据类型

   基本数据类型是Java语言预定义的，它们的大小和值范围是固定的。Java中有8种基本数据类型：

   - 整数类型：
     - byte: 占1个字节（8位），取值范围是-128到127。
     - short: 占2个字节（16位），取值范围是-32,768到32,767。
     - int: 占4个字节（32位），取值范围是-2^31到2^31-1。
     - long: 占8个字节（64位），取值范围是-2^63到2^63-1。
   - 浮点类型：
     - float: 占4个字节（32位），提供单精度浮点数，大约有6-7位有效数字。
     - double: 占8个字节（64位），提供双精度浮点数，大约有15位有效数字。
   - 字符类型：
     - char: 占2个字节（16位），用于存储Unicode字符，取值范围是'\u0000'到'\uffff'。
   - 布尔类型：
     - boolean: 不占固定字节数，但通常认为是1个字节，只有两个值：true 和 false。

2. 引用数据类型

   - 类（Class）：用户定义的类，以及Java API中的所有类，如String、ArrayList等。
   - 接口（Interface）：定义一组方法签名，但不包含具体实现的类型。
   - 数组：可以存储同类型元素的集合，如int[]、String[]等。

**值与引用的区别**

基本数据类型的变量直接存储值，而引用数据类型的变量存储的是对象的内存地址，也就是说，它们指向实际对象在内存中的位置。当你创建一个引用类型变量时，你实际上是创建了一个指向对象的引用，而不是对象本身。

**注意事项**

- Java中的基本数据类型不是对象，因此不能直接调用方法。为了在基本类型上使用方法，通常需要将其包装成对应的包装类，如Integer、Double等。
- 基本数据类型用（==）比较，引用数据类型使用equals()方法比较。
- 引用数据类型可以为null，表示没有引用任何对象。

#### 包装类

在Java中，为了方便基本数据类型与对象之间的转换，Java提供了8个对应的包装类（Wrapper Classes）：

1. Integer - 整数
2. Character - 字符
3. Boolean - 布尔
4. Byte - 字节
5. Short - 短整数
6. Long - 长整数
7. Float - 单精度浮点数
8. Double - 双精度浮点数

这些包装类提供了将基本数据类型转换为对象的方法，如valueOf()，同时也支持自动装箱（Autoboxing）和自动拆箱（Unboxing）特性，使得基本类型与对象之间可以无缝转换。

#### 缓存池

对于某些包装类，特别是Integer、Character、Byte、Short和Long，Java提供了一个缓存池，以优化这些类的valueOf()方法。这个缓存池是为了存储一定范围内（通常是-128到127）的常量对象，这样在多次请求相同值的包装对象时，可以避免重复创建对象，提高性能。

- 对于Integer，缓存池的范围默认是-128到127，但可以通过JVM启动参数-XX:AutoBoxCacheMax来调整上界。
- 对于Character，由于字符的取值范围较小（0到65535），缓存池通常覆盖了整个范围。
- 对于Byte、Short和Long，缓存池也覆盖了-128到127的整数值。

例如，如果你连续两次调用Integer.valueOf(5)，第二次调用会从缓存中直接获取对象，而不会创建新的Integer实例。这在处理大量重复数值时特别有用，尤其是在循环和集合操作中。

```java
Integer num1 = Integer.valueOf(5);
Integer num2 = Integer.valueOf(5);
System.out.println(num1 == num2); // 输出: true，因为它们共享同一个对象
```

需要注意的是，Float和Double没有类似的缓存池，因为它们的精度和内存占用较大，不适用于这种优化策略。而Boolean也没有缓存池，因为true和false通常被视为两个独立的值，不需要额外的优化。

### String类

Java的String类型是一个用于表示文本的不可变（Immutable）对象，是Java编程中最常用的数据类型之一。以下是关于String的一些关键点：

1. 不可变性

   一旦创建了一个String对象，它的内容就不能被修改。任何对String的操作，如拼接、替换字符等，都会返回一个新的String对象，而原来的对象保持不变。

2. 创建方式

   - 字面量赋值：最常见的创建方式，如 String str = "Hello, World!";。这种情况下，如果字符串内容相同，JVM会自动复用已经存在的字符串对象（字符串常量池机制）。
   - 构造函数：通过new关键字创建，如 String str = new String("Hello, World!");。这种方式每次都会创建新的对象，即使内容相同，也不会自动利用字符串常量池。

3. 字符串常量池（String Pool）

   Java虚拟机维护了一个特殊的存储区域，称为字符串常量池，用于存储字符串字面量。当使用字面量方式创建字符串时，JVM首先会在池中查找是否存在内容相同的字符串，如果有，则直接引用该对象；如果没有，则创建新的字符串对象并放入池中。

4. 字符数组与String转换

   - 可以通过字符数组创建String，如 char[] charArray = {'H', 'e', 'l', 'l', 'o'}; String str = new String(charArray);
   - 也可以将String转换为字符数组，使用toCharArray()方法。

5. 常用方法

   - length()：返回字符串长度。
   - concat(String str)：将指定的字符串连接到此字符串的末尾。
   - substring(int beginIndex, int endIndex)：返回字符串的一个子字符串。
   - equals(Object anObject)：比较字符串内容是否相等
   - equalsIgnoreCase(String anotherString)：忽略大小写比较字符串内容是否相等。
   - replace(char oldChar, char newChar)：将字符串中的某个字符全部替换为另一个字符。
   - 还有很多其他方法，如split(String regex)、trim()、toLowerCase()、toUpperCase()等。

6. 字符编码

   String在内部使用Unicode字符集进行编码，支持国际化的文本表示。

7. 性能考虑

   由于String的不可变性，频繁修改字符串的操作会导致大量临时对象的产生，影响性能。在这种场景下，推荐使用StringBuilder或StringBuffer类，它们提供了可变的字符串缓冲区，适用于字符串的拼接和修改操作。

8. 字符串比较

   - 使用==比较字符串时，比较的是对象引用是否相同，而不是内容是否相等。
   - 应使用equals()方法来比较字符串内容的相等性。

**源码：**

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {

    private final char value[];

    private int hash; // Default to 0

    // 构造函数
    public String() {
        this.value = ""..toCharArray();
    }

    public String(char[] value) {
        this.value = Arrays.copyOf(value, value.length);
    }

    public String(String original) {
        this.value = original.value.clone();
    }

    // 获取字符串长度
    public int length() {
        return value.length;
    }

    // 判断字符串是否为空
    public boolean isEmpty() {
        return value.length == 0;
    }

    // 字符串比较
    public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0 && v1[i] == v2[i])
                    i++;
                return n == 0;
            }
        }
        return false;
    }

    // 返回字符串的哈希码
    public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }

    // ... 其他方法（如substring、indexOf、replace等）
}
```

实际的String类源码还包括许多其他方法，如compareTo用于比较字符串的自然顺序，charAt用于获取指定索引处的字符，substring用于提取子字符串，indexOf和lastIndexOf用于查找子字符串，以及getBytes用于将字符串转换为字节数组等。此外，String类还实现了Serializable接口，意味着String对象可以序列化和反序列化。Comparable接口的实现使得String对象可以与其他String进行比较。

#### StringBuffer、StringBuilder

StringBuffer和StringBuilder都是Java中用于处理字符串的可变序列的类，它们提供了在字符串中进行修改的方法，如追加、插入、删除字符等，而不会像String那样每次操作都创建新的字符串对象。它们的主要区别在于线程安全性：

1. StringBuffer
   - 线程安全：StringBuffer是线程安全的，它的方法都用synchronized关键字修饰，这意味着在多线程环境下，多个线程可以安全地访问和修改同一个StringBuffer对象，而不会导致数据不一致的问题。线程安全的特性使得StringBuffer在并发访问时更加可靠，但这也带来了性能上的开销，因为同步操作会增加额外的时间成本。
2. StringBuilder
   - 非线程安全：StringBuilder是StringBuffer的非线程安全版本，它没有同步锁，因此在单线程环境下性能优于StringBuffer。由于省去了同步操作，StringBuilder在字符串拼接等操作上通常更快。但如果在多线程环境下不加控制地使用StringBuilder，可能会导致数据竞争和不一致性问题。

共同点

- 可变性：两者都是可变的，即它们的内部维护了一个字符数组，可以直接修改数组中的元素来改变字符串内容。
- 方法相似：它们提供的API非常相似，包括append()、insert()、delete()、reverse()等方法，用于操作字符串序列。
- 构造函数：都可以通过字符数组、字符串或初始容量来创建实例。

使用场景

- 如果你的代码在单线程环境中运行，使用StringBuilder可以获得更好的性能。
- 如果你需要在多线程环境下操作字符串，那么应该使用StringBuffer以保证线程安全。
- 在进行大量的字符串拼接操作时，优先考虑使用StringBuilder或StringBuffer而不是直接使用+运算符，以避免产生过多的临时字符串对象，进而提高性能。

示例代码

```java
// 使用StringBuilder
StringBuilder sb = new StringBuilder();
sb.append("Hello, ");
sb.append("World!");
System.out.println(sb.toString()); // 输出: Hello, World!

// 使用StringBuffer
StringBuffer sbf = new StringBuffer();
sbf.append("Java ");
sbf.append("is ");
sbf.append("cool.");
System.out.println(sbf.toString()); // 输出: Java is cool.
```

#### 字符串常量池

字符串常量池（String Constant Pool 或 String Pool）是Java虚拟机（JVM）为了提升内存使用效率和字符串操作的性能而设计的一个特殊存储区域。它主要存储字符串字面量（即通过双引号定义的字符串）以及编译期间确定的字符串。下面详细介绍字符串常量池的工作原理和特点：

1. 存储位置

   - 在Java 7及之前版本中，字符串常量池位于永久代（PermGen space）中。
   - 从Java 8开始，永久代被元空间（Metaspace）取代，字符串常量池则移动到了堆内存（Heap）中。

2. 工作原理

   - 创建与复用：当程序创建一个字符串字面量时，JVM首先会检查字符串常量池中是否已经存在一个完全相同的字符串。如果存在，则直接复用该字符串的引用，避免了重复创建；如果不存在，则在池中创建一个新的字符串实例，并返回该实例的引用。
   - 编译期与运行期：编译器在编译阶段会将出现在代码中的字符串字面量加入到常量池中。而在运行时，如果通过String类的构造方法创建字符串，JVM也会尝试将其加入到常量池中（例如，使用new String("abc")时，如果常量池中已存在"abc"，则不会将新创建的对象加入池中，但会创建一个新的对象在堆上）。

3. 内存优化

   - 减少内存占用：通过共享字符串实例，减少了大量相同字符串的重复存储，节省了内存空间。
   - 提高性能：直接引用常量池中的字符串比创建新的字符串对象要快，因为无需分配新的内存和初始化对象。

4. intern()方法

   - intern() 方法允许开发者手动将一个字符串实例添加到常量池中。如果池中已存在相同内容的字符串，则返回池中字符串的引用；如果不存在，则将该字符串添加到池中，并返回其引用。

   示例

   ```java
   String s1 = "Hello";
   String s2 = "Hello";
   String s3 = new String("Hello");
   String s4 = s3.intern();
   
   System.out.println(s1 == s2); // true，s1和s2指向常量池中的同一个对象
   System.out.println(s1 == s3); // false，s3是堆上的新对象
   System.out.println(s1 == s4); // true，s4通过intern()方法指向了常量池中的对象
   
   ```

注意事项

- 对于字符串拼接操作，使用+运算符会创建新的字符串实例，即使最终结果可能与池中已有字符串相同。因此，在大量拼接操作时，推荐使用StringBuilder或StringBuffer以避免不必要的对象创建。
- 字符串常量池的优化主要针对字面量字符串，对于通过构造函数动态创建的字符串，除非显式调用intern()，否则不会自动加入常量池。

### Object类

Object类是Java中所有类的根类，位于java.lang包下。每个Java类都直接或间接地继承自Object，即使没有明确声明，也会默认继承。Object类提供了一些基础的方法和属性，这些方法和属性是所有Java对象共有的。以下是Object类中的一些重要方法：

1. toString():
   - 返回一个表示该对象的字符串，通常包括类名和对象的哈希码。默认的实现会返回类似于"java.lang.Object@12345678"的形式，其中12345678是对象的十六进制哈希码。大多数类会重写这个方法以返回更有意义的字符串表示。
2. equals(Object obj):
   - 检查此对象与指定对象是否“相等”。默认的实现基于引用比较，即如果两个对象是同一个对象（即引用相同），则返回true。大多数类会重写equals()以根据类的特定逻辑判断两个对象是否在逻辑上相等。
3. hashCode():
   - 返回此对象的哈希码值，用于哈希表（如HashMap）中的快速查找。默认的实现基于对象的内存地址，但为了保持equals()和hashCode()的一致性，重写equals()时通常也需要重写hashCode()。
4. clone():
   - 创建并返回此对象的一个副本。默认实现是浅复制，即只复制对象本身，不复制对象可能引用的其他对象。深复制（复制对象及其引用的所有对象）需要在子类中实现。
5. finalize():
   - 当垃圾收集器确定不存在对该对象的更多引用时，由垃圾收集器调用。通常用于清理资源，但不推荐依赖此方法来释放资源，因为其执行时机不确定。
6. getClass():
   - 返回此对象的运行时类，即实际类的Class对象。
7. wait(), notify(), notifyAll():
   - 这些方法用于线程的同步和协作。wait()使当前线程等待，直到其他线程调用notify()或notifyAll()唤醒它。这些方法必须在同步块或方法中调用，否则会抛出IllegalMonitorStateException。

### final关键字

Java中的final关键字是一个非常重要的概念，它用于声明一个不可改变的实体。final可以用来修饰以下三类编程元素：

1. 常量（Final Variables）：

   - 当final用于修饰一个变量时，这意味着一旦给该变量赋值后，就不能再改变其值。对于基本类型的变量，这意味着值不能修改；对于引用类型的变量，这意味着引用本身不能改变（即不能指向另一个对象），但对象的内容（如果对象是可变的）仍然可以修改。

   ```java
      final int CONSTANT = 42; // 基本类型，值不可变
      final List<String> list = new ArrayList<>(); // 引用类型，引用不可变，但列表内容可变
   ```

   在类的静态初始化块中，或者在变量声明时直接赋值，可以初始化final变量。

2. 方法（Final Methods）：

   - 如果一个方法被声明为final，则该方法不能在子类中被重写（override）。这有助于确保代码的封装性和行为的一致性。

   ```java
      public final void myMethod() {
          // 方法体
      }
   ```

3. 类（Final Classes）：

   - 当final应用于类时，该类不能被其他类继承。这有助于防止意外的修改和保持类的完整性。例如，String类就是final的，所以不能有自定义的String子类。

   ```java
      public final class MyClass {
          // 类体
      }
   ```

4. 匿名内部类：

   - final还可以隐式地应用到匿名内部类的每个未声明为final的外部局部变量，以便内部类可以访问它们。如果想要在内部类中使用外部的局部变量，必须将它们声明为final。

5. 枚举（Enums）

   - 枚举类型默认就是final的，不能被继承，且枚举常量的值也是不可变的。

使用final的关键在于确保某些东西不会在程序运行期间发生变化，从而提高代码的可预测性、安全性和效率。同时，final也是Java中的一个核心概念，尤其是在设计模式和并发编程中，如final常量用于线程安全，以及final方法和类在单例模式中的应用。

### static关键字

static关键字在Java中扮演着重要的角色，它主要用于声明类级别的成员，而不是对象级别的成员。以下是static关键字的主要用途和特点：

1. 静态变量（Static Fields）：

   - static修饰的变量称为静态变量或类变量，它们属于类而非任何特定的对象。这意味着所有类的实例都可以共享同一个静态变量的副本，而不是每个实例都有自己独立的副本。

   ```java
      public class MyClass {
          static int count = 0; // 全局计数器
      }
   ```

2. 静态方法（Static Methods）：

   - static修饰的方法称为静态方法，它们可以直接通过类名调用，无需创建对象。静态方法不能访问类的非静态成员，因为非静态成员依赖于特定的对象实例。

   ```java
      public class MathUtils {
          public static double square(double num) {
              return num * num;
          }
      }
      MathUtils.square(5); // 直接通过类名调用
   ```

3. 静态初始化块（Static Initializers）：

   - 静态初始化块用于在类加载时初始化静态变量。这些块在类加载时执行一次，按照它们在类中的顺序。

   ```java
      public class MyClass {
          static {
              System.out.println("Class loaded.");
          }
      }
   ```

4. 静态内部类（Static Nested Classes）：

   - static可以用于内部类，创建静态内部类。静态内部类不持有对外部类的引用，因此可以在没有外部类实例的情况下创建。而普通内部类（非静态）则需要一个外部类的实例。

   ```java
      public class OuterClass {
          public static class InnerClass {
              // ...
          }
      }
   ```

5. 单例模式：

   - static在实现单例模式时也很常见，用于确保类只有一个实例，并提供全局访问点。

使用static关键字时，需要注意的是，静态成员不依赖于对象，因此它们不能访问非静态成员，因为非静态成员与对象实例相关联。此外，过度使用static可能导致代码难以测试和维护，因为它可能导致紧耦合和全局状态。

### JIT即时编译

JVM（Java虚拟机）在执行Java字节码时，采用了两种主要的执行策略：解释执行和即时编译（Just-In-Time Compilation, JIT）。在现代JVM中，这两种策略往往不是孤立使用的，而是结合在一起，形成了一种混合模式，即解释执行与编译并行的策略，以达到更好的性能表现。这种机制的实现原理大致如下：

1. 解释执行（Interpretation）
   - 解释执行是最直接的方式，JVM逐条读取字节码指令，然后直接执行每一条指令。这种方式启动速度快，但因为每次执行都需要翻译，所以效率相对较低。
   - 快速启动，无需预编译，对代码更改敏感，易于调试。
2. 即时编译（JIT Compilation）
   - 原理：JIT编译器监控程序运行时的行为，识别出那些经常被执行的“热点”代码（Hot Spot Code），并将这些代码从字节码转换成本地机器码（Native Code）存储起来。之后，当再次遇到这部分代码时，可以直接执行高效的本地机器码，避免了解释执行的开销。
   - 优势：显著提高热点代码的执行效率，减少运行时的解释开销。
3. 解释执行与编译并行的实现
   - 混合模式：JVM采用了一种称为“混合模式”的执行策略，即同时使用解释器和JIT编译器。程序启动初期，主要使用解释器快速加载和执行代码，保证了快速启动。与此同时，JIT编译器在后台开始监控和分析代码执行情况，识别热点代码。
   - 编译策略：一旦发现热点代码，JIT编译器会将其编译成本地机器码，并在后续执行中替换掉解释执行的部分。这个过程对于用户来说是透明的，且可以显著提升执行效率。
   - 编译层次：某些JVM（如HotSpot）还支持多层编译策略，即对热点代码进行多次编译，逐步优化，从快速但简单优化的编译到耗时较长但高度优化的编译，以此达到最佳性能平衡。
   - 编译与执行并行：在多核处理器环境下，JIT编译可以在一个或多个单独的线程中进行，与解释执行的线程并行工作，这样既不会阻塞程序的正常执行，又能充分利用硬件资源加速编译过程。

综上所述，JVM通过解释执行与即时编译并行的策略，实现了快速启动和高效运行的双重目标，是Java能够广泛应用于各种场景的关键技术之一。

## 异常

在Java程序运行过程中，不可避免的会出现各种错误，异常处理就是对各种异常情况进行处理。

在Java当中，异常是一种class，各种异常类的继承关系如下：

1. Throwable类：
   - 所有异常和错误的顶级父类，它有两个直接的子类：Exception和Error
2. Exception类：
   - 代表程序可以合理的预期的异常情况，这些异常通常可以通过适当的恢复机制处理
   - Exception 类又分为两个主要类别：
     - 受检异常（Checked Exceptions）：
       - 子类通常继承自 Exception 类，但不是 RuntimeException 或其子类
       - 示例：IOException, SQLException, ClassNotFoundException
       - 这些异常在编译时需要被处理，要么通过 try-catch 语句块捕获，要么在方法签名中用 throws 关键字声明。
     - 非受检异常（Unchecked Exceptions）：
       - 子类直接或间接继承自RunTimeException类
       - 示例：NullPointerException, ArrayIndexOutOfBoundsException, IllegalArgumentException
       - 这些异常在编译时不强制要求处理，但在运行时如果抛出，程序会中断
3. Error类：
   - 用于表示严重的问题，这些问题通常与JVM状态有关，应用程序通常无法恢复
   - 示例：OutOfMemoryError（内存溢出）, StackOverflowError（堆栈溢出）, VirtualMachineError（虚拟机错误）
   - 错误通常不被捕获，因为它们表示系统级别的问题，通常意味着程序无法继续执行

<img src="C:\Users\richa\AppData\Roaming\Typora\typora-user-images\image-20240507211853704.png" alt="image-20240507211853704" style="zoom:80%;" />

异常处理中的关键字包括：

- **try**：用于包含可能抛出异常的代码。
- **catch**：用于处理异常。它捕获try块抛出的异常。
- **finally**：用于执行重要的代码，比如关闭数据库连接，无论异常是否被处理。
- **throw**：用于显式地抛出异常。
- **throws**：用于声明方法可能抛出的异常。

在这个例子中，如果文件不存在，FileReader构造函数会抛出FileNotFoundException，然后控制流进入catch块。无论是否发生异常，finally块都会关闭文件，确保资源得到正确释放：

```java
try {
    // 试图打开一个文件
    File file = new File("example.txt");
    FileReader fr = new FileReader(file);
} 
catch (FileNotFoundException e) {
    System.out.println("File not found: " + e.getMessage());
}
finally {
    // 无论是否发生异常，都会关闭文件
    if (fr != null) {
        try {
            fr.close();
        } catch (IOException ioe) {
            System.err.println("Error closing file: " + ioe.getMessage());
        }
    }
}

```

**注意事项：**

- 只捕获你可以处理的异常。
- 使用特定的异常而不是捕获通用的异常。
- 在finally块中关闭资源。
- 避免直接捕获Throwable或Exception，因为这可能会隐藏关键错误。

### 自定义异常处理类

```java
/**
 * MyCustomRuntimeException 是一个继承自 RuntimeException 的自定义异常类，
 * 用于表示应用程序中特定的运行时错误情况。
 */
public class MyCustomRuntimeException extends RuntimeException {

    public MyCustomRuntimeException() {
        super();
    }
    
    public MyCustomRuntimeException(String message) {
        super(message);
    }
    
    public MyCustomRuntimeException(String message, Throwable cause) {
        super(message, cause);
    }
   
    public MyCustomRuntimeException(Throwable cause) {
        super(cause);
    }
}

```

### ExceptionHandler

在Spring MVC或Spring Boot框架中，自定义ExceptionHandler是一种优雅地处理和管理应用程序中可能出现的异常的方式。相当于定义了一个全局异常处理类，只要是在全局异常类里规定了处理逻辑的异常类型，就不需要使用try-catch捕获处理了。

@ExceptionHandler是Spring框架提供的一个注解，它可以放在控制器类的方法上，用于声明该方法应该处理哪些类型的异常。当控制器中的其他方法抛出匹配的异常时，Spring会自动调用这个处理方法，而不是按照默认的错误处理流程执行。

解释几个问题：

1. 注解位置：@ExceptionHandler通常与@Controller或@RestControllerAdvice一起使用。@ControllerAdvice是全局的异常处理，它会应用到整个应用或特定包下的所有控制器。
2. 异常类型：注解后面可以指定一个或多个异常类，表示该方法将处理这些类或其子类的实例。例如：@ExceptionHandler(BadCredentialsException.class)表示处理BadCredentialsException
3. 处理逻辑：处理方法通常会返回一个视图名、模型对象或者HTTP响应。在返回的响应中，你可以包含错误代码、错误消息等信息，以帮助客户端理解发生了什么问题
4. 返回类型：处理方法的返回类型通常是ModelAndView、String（视图名）、或者ResponseEntity，以便构建和返回HTTP响应
5. 通用异常处理：可以使用@ExceptionHandler(Throwable.class)或@ExceptionHandler(Exception.class)来捕获所有未被其他处理方法捕获的异常，这是一种全局的异常处理器

通过这种方式，开发者可以创建一个整洁的异常处理层，确保所有异常都能得到适当的处理，同时保持了良好的用户体验，因为可以定制返回给用户的错误信息，而不是显示系统的错误堆栈。

示例代码：

```java
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;

@ControllerAdvice
public class CustomExceptionHandler {

    /**
     * 处理NullPointerException
     */
    @ExceptionHandler(NullPointerException.class)
    public ResponseEntity<String> handleNullPointerException(NullPointerException ex) {
        return new ResponseEntity<>("NullPointerException occurred: " + ex.getMessage(), HttpStatus.BAD_REQUEST);
    }

    /**
     * 处理自定义异常CustomException
     */
    @ExceptionHandler(CustomException.class)
    public ResponseEntity<String> handleCustomException(CustomException ex) {
        return new ResponseEntity<>("CustomException occurred: " + ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }

    /**
     * 全局异常处理，处理未被其他异常处理器捕获的异常
     */
    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleAllOtherExceptions(Exception ex) {
        return new ResponseEntity<>("An unexpected error occurred: " + ex.getMessage(), HttpStatus.INTERNAL_SERVER_ERROR);
    }
}

// 自定义异常类
class CustomException extends Exception {
    public CustomException(String message) {
        super(message);
    }
}

```

在这个例子中，CustomExceptionHandler是一个全局异常处理器，它使用@ControllerAdvice注解来声明这是一个应用于所有控制器的异常处理器。@ExceptionHandler注解指定了处理特定异常的方法。如果发生匹配的异常，相应的处理方法将被调用，并返回一个包含错误信息的ResponseEntity。最后，有一个处理所有未被其他处理器捕获的异常的通用方法。

### try-with-resources

try-with-resources是Java 7引入的一个新特性，它旨在简化资源的打开和关闭，尤其是那些实现了AutoCloseable接口的资源，如文件流、数据库连接等。try-with-resources语句确保在try语句块执行完毕后，无论是否发生异常，都会调用资源的close()方法进行关闭，从而避免资源泄露。

try-with-resources的基本语法如下：

```java
try (ResourceType resource = createResource()) {
    // 使用资源进行操作
} catch (ExceptionType1 e1) {
    // 处理ExceptionType1
} catch (ExceptionType2 e2) {
    // 处理ExceptionType2
} finally {
    // 可选的finally块，但通常不需要
}

```

- ResourceType是实现了AutoCloseable接口的类类型。
- createResource()是创建资源的对象实例，可以是构造函数调用或其他创建方法。
- 在try括号内，我们声明并初始化资源对象。
- 在try块中，我们可以使用这个资源进行操作。
- 如果资源在使用过程中抛出异常，控制权将立即转移到相应的catch块，如果没有匹配的catch块，异常将被抛出。
- 不需要finally块来关闭资源，因为try-with-resources会自动调用close()方法。

这个特性使得资源管理更加简洁、安全，避免了忘记在finally块中关闭资源可能导致的问题。

## JVM（Java Virtual Machine）

JVM是Java程序的运行时环境，它直接与操作系统进行交互。

<img src="https://static001.geekbang.org/infoq/da/da0380a04d9c04facd2add5f6dba06fa.png" alt="img" style="zoom: 80%;" />

### 程序执行流程

Java程序在JVM（Java虚拟机）中的执行流程可以分为几个关键步骤，以下是详细的执行流程：

1. **源代码编写：**开发者使用Java语言编写程序代码，保存为.java文件。

2. **编译：**使用javac编译器将.java文件编译为字节码，即.class文件。这个过程将高级的Java代码转换为JVM可以理解的低级指令集。

3. **类加载：**当程序运行时，JVM会通过类加载器（ClassLoader）将需要的.class文件加载到内存中。类加载包含以下阶段：

   - 加载：找到并读取类文件
   - 验证：确保类文件符合JVM规范
   - 准备：为类的静态变量分配内存，并赋予默认初始值
   - 解析：将常量池中的符号引用转换为直接引用
   - 初始化：执行类的静态初始化代码，包括静态变量赋值和静态初始化块

4. **执行引擎：**

   加载和初始化完成后，JVM的执行引擎开始执行字节码。

   - 解释执行：最开始，字节码可能被逐条解释执行
   - 即时编译（JIT）：热点代码（经常执行的代码段）会被JVM的即时编译器识别，并编译成本地机器码，以提高执行效率

### 类加载机制

**类的生命周期**

类加载的过程包括了`加载`、`验证`、`准备`、`解析`、`初始化`五个阶段。在这五个阶段中，`加载`、`验证`、`准备`和`初始化`这四个阶段发生的顺序是确定的，*而`解析`阶段则不一定，它在某些情况下可以在初始化阶段之后开始，这是为了支持Java语言的运行时绑定(也成为动态绑定或晚期绑定)*。另外注意这里的几个阶段是按顺序开始，而不是按顺序进行或完成，因为**这些阶段通常都是互相交叉地混合进行的，通常在一个阶段执行的过程中调用或激活另一个阶段。**

![img](https://pdai.tech/images/jvm/java_jvm_classload_2.png)

**类的加载：查找并加载类的二进制数据**

加载过程，虚拟机完成三件事情：

- 通过类的全限定名（包名+类名）根据类加载策略找到对应的.class文件
- 将这个字节流所代表的静态存储结构转化为方法区的运行时数据结构（类的元数据）
- 在Java堆中生成一个代表这个类的java.lang.Class对象，作为对方法区中这些数据的访问入口

![img](https://pdai.tech/images/jvm/java_jvm_classload_1.png)

**验证：确保被加载类的正确性**

在加载过程中，JVM会进行字节码验证，以确保字节码文件符合Java虚拟机规范，防止恶意代码注入。验证包括：

- 文件格式验证：检查文件头、魔数、版本号等是否正确
- 元数据验证：验证类的继承关系、接口、字段、方法等是否合法
- 字节码验证：确保执行流程的安全性，检查指令序列是否存在非法操作
- 符号引用验证：确认引用的类、接口、字段和方法是否有效，能否在运行时解析

**准备：为类的静态变量分配内存，并将其初始化为默认值**

在方法区（Java 8之前的永久代或之后的元空间）中为类的静态变量分配内存，并赋予它们的默认初始值。例如，int类型默认为0，引用类型默认为null。

**解析：把类中的符合引用转换为直接引用**

将字节码文件中的符号引用（如类名、方法名和字段名）转换为直接引用，这些直接引用可以直接定位到内存中的实际对象或方法。

**初始化：**

JVM负责对类进行初始化，主要对类变量进行初始化。在Java中对类变量进行初始值设定有两种方式:

- 声明类变量是指定初始值
- 使用静态代码块为类变量指定初始值

​	**JVM初始化步骤**

- 假如这个类还没有被加载和连接，则程序先加载并连接该类
- 假如该类的直接父类还没有被初始化，则先初始化其直接父类
- 假如类中有初始化语句，则系统依次执行这些初始化语句

​	**类初始化时机**: 只有当对类的主动使用的时候才会导致类的初始化，类的主动使用包括以下六种:

- 创建类的实例，也就是new的方式
- 访问某个类或接口的静态变量，或者对该静态变量赋值
- 调用类的静态方法
- 反射(如Class.forName("com.pdai.jvm.Test"))
- 初始化某个类的子类，则其父类也会被初始化
- Java虚拟机启动时被标明为启动类的类(Java Test)，直接使用java.exe命令来运行某个主类

### 内存模型

<img src="https://pdai.tech/images/jvm/jvm/0082zybply1gc6fz21n8kj30u00wpn5v.jpg" alt="jvm-framework" style="zoom: 67%;" />

Java虚拟机（JVM）的内存模型是Java程序运行时内存分配和管理的架构，它包括了多个区域，这些区域各自有不同的功能和生命周期。以下是一个简要的概述：

1. 程序计数器（Program Counter Register）：
   - 每个线程都有自己的程序计数器，它是一个非常小的内存区域，保存了线程当前正在执行的虚拟机指令的地址。在多线程环境下，线程切换时，这个计数器用于记录下次应该执行的指令。
2. 虚拟机栈（Java Virtual Machine Stack）：
   - 与线程相关，每个线程都有自己的虚拟机栈，用于存储方法调用的局部变量表、操作数栈、动态链接和方法出口等信息。每当方法被调用，就会在栈上创建一个新的栈帧，方法执行完毕后，栈帧会被弹出。
3. 本地方法栈（Native Method Stack）：
   - 类似于虚拟机栈，但用于支持Java Native Interface (JNI)调用的本地（非Java）方法。它为每个线程存储本地方法调用的状态。
4. Java 堆（Heap）：
   - 线程共享的内存区域，是Java对象的主要存储场所。所有类的实例和数组都分配在堆中。垃圾收集器（GC）主要负责管理这块内存，进行对象的创建和回收。
5. 方法区（Method Area）（Java 8及以后版本称为元空间Metaspace）：
   - 也称为非堆（Non-Heap），存储类的元数据，如类的结构信息（如方法、字段、常量池等）。在Java 8中，元空间不再限制在固定大小的内存区域，而是使用操作系统内存。
6. 运行时常量池（Runtime Constant Pool）：
   - 属于方法区的一部分，存储类文件中的常量、字符串、符号引用等。在Java 8中，运行时常量池被移到了堆中。
7. 直接内存（Direct Memory）（非JVM规范定义，但常见于实际应用）：
   - 不是JVM规范中定义的内存区域，但通过NIO（New Input/Output）库可以直接在堆外分配内存，以提高性能。这部分内存不受垃圾收集器管理，需要手动释放。

JVM内存模型的设计旨在平衡内存的效率、速度和可用性，通过垃圾收集器来自动管理内存，减少程序员对内存管理的负担。不同的JVM实现可能会有细微差别，但基本概念是相同的。

### 垃圾回收机制

Java虚拟机（JVM）的垃圾回收机制（Garbage Collection, GC）是自动管理内存的一种机制，它自动追踪并回收不再使用的对象所占用的内存空间，从而减轻了开发者的内存管理负担。JVM的垃圾回收机制主要包括以下几个核心部分：

1. 对象的创建和可达性分析

   - 对象创建：新创建的对象通常分配在堆内存中。
   - 可达性分析：JVM通过可达性分析来判断对象是否可回收。它从一组称为GC Roots的根对象出发，沿着引用链遍历对象图。如果一个对象到GC Roots没有任何引用路径可达，那么这个对象就被认为是不可达的，可以被回收。

2. 回收算法

   JVM中的垃圾回收算法主要有以下几种：

   - 引用计数法：每个对象有一个引用计数器，当对象被引用时计数器加1，引用失效时减1。当计数器为0时，对象被视为可回收。但这种方法无法解决循环引用的问题，因此在现代JVM中不常用。
   - 标记-清除算法：分为两步，首先标记所有从GC Roots可达的对象，然后清除未标记的对象。此算法简单，但会导致内存碎片化。
   - 复制算法：将内存分为两个相等的区域，每次只使用其中一个区域。当该区域满时，将存活的对象复制到另一个区域，然后清空使用过的区域。此算法解决了碎片化问题，但需要额外的内存空间。
   - 标记-整理算法：首先标记所有存活对象，然后将所有存活对象移动到一端，直接清理掉边界外的内存空间。适用于老年代，解决了碎片化问题。
   - 分代收集算法：将堆内存分为年轻代（Young Generation）和老年代（Old Generation），年轻代又分为Eden区、Survivor区（通常有两个，From和To）。根据对象的生命周期特点，采取不同的回收策略。年轻代通常使用复制算法，老年代使用标记-清除或标记-整理算法。

3. 垃圾收集器

   JVM提供了多种垃圾收集器，不同版本和实现可能有所差异，常见的有：

   - Serial GC：单线程收集器，适合单CPU环境，简单高效，但会暂停应用线程。
   - Parallel GC（也称吞吐量优先收集器）：多线程收集器，适合多CPU环境，追求高吞吐量，但STW（Stop The World）事件较长。
   - CMS（Concurrent Mark Sweep）：以最小化停顿时间为目标的收集器，大部分工作与用户线程并发进行，但可能产生内存碎片。
   - G1（Garbage First）：面向服务器的垃圾收集器，将堆内存划分为多个大小相等的区域，通过预测性地选择回收价值高的区域来减少停顿时间，同时兼顾吞吐量和延迟。
   - ZGC（Z Garbage Collector） 和 Shenandoah：都是为了实现低延迟垃圾回收而设计的，几乎可以做到停顿时间不超过10ms，特别适合对延迟敏感的应用。

## 反射

Java反射机制是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用这个对象的所有属性和方法；这种动态获取类信息和调用对象的功能称为Java语言的反射机制。

### 反射基础

RTTI（运行时类型识别）

RTTI允许程序在运行时确定对象的实际类型，这是多态性的一个关键方面。Java中的RTTI主要通过以下两种方式实现：

1、类型转换：基本形式的RTTI发生在使用instanceof关键字检查对象是否属于特定类或其子类时，以及显式类型转换时。例如：

```java
   Object obj = new String("Hello");
   if (obj instanceof String) {
       String str = (String) obj;
       // 此时可以安全地使用str调用String类的方法
   }
```

2、Class类：每个类在Java中都有一个对应的Class对象，它包含了关于类的元数据，如类名、包名、父类、实现的接口、构造器、方法和字段等。你可以通过getClass()方法获取对象的Class对象，进而获取类的详细信息。例如：

```java
   String str = "Hello";
   Class<?> clazz = str.getClass(); // 获取Class对象
   System.out.println(clazz.getName()); // 输出实际类型名称
```

### Class类

Class类，Class类也是一个实实在在的类，存在于JDK的java.lang包中。Class对象是对方法区（元空间）的类的元数据的包装，它是反射机制的基石，通过它这个窗口可以获得不同数据类型的信息

- 类（class and enum）
- 接口（interface and annotation）
- 数组
- 基本类型boolean，byte，char，short，int，long，float，double和关键字void的包装类

其关键特性与结构如下：

1. 继承关系：
   - Class 类继承自 Object 类，这意味着所有类都是 Class 类的实例
   - 它也是 GenericDeclaration, Type, AnnotatedElement 的子类或实现类，表明它可以携带泛型信息、类型信息以及注解
2. 静态成员变量：
   - public static final Class<?>[] EMPTY_CLASS_ARRAY = {};	一个空的 Class 数组，常用于避免创建空数组的开销
   - private static final int SYNTHETIC = 0x00001000;               标志位，表示类或成员是编译器生成的（比如桥接方法）
3. 构造方法：
   - Class 类的构造方法是私有的，确保只能由 JVM 创建 Class 实例
4. 核心方法：
   - public String getName()：返回此 Class 对象所表示的实体（类、接口、数组类、基本类型或void）的全限定名
   - public Constructor<T>[] getConstructors()：返回一个包含此类所有公共构造方法的数组
   - public Method[] getMethods()：返回一个包含此类所有公共（包括继承的）方法的数组
   - public Field[] getFields()：返回一个包含此类所有可访问公共字段的数组
   - public Class<? super T> getSuperclass()：返回表示此 Class 所表示的类的超类的 Class
   - protected Constructor<T> getConstructor(Class<?>... parameterTypes)：返回一个 Constructor 对象，它反映此 Class 对象所表示的类的指定公共构造方法
   - public T newInstance()：使用此 Class 对象所表示的类的默认构造方法创建该类的新实例
5. Native方法：
   - private static native void registerNatives();：这是一个本地方法，用于在类加载时注册一些本地方法到 JVM 中，通常用于与底层系统交互
6. 初始化块：
   - static { registerNatives(); }：确保在类加载时执行本地方法注册

```java
public class Class<T> extends Object
    implements GenericDeclaration, Type, AnnotatedElement {

    private static native void registerNatives();

    // 省略构造方法，因为它们是私有并且由JVM管理

    public String getName() {
        return name;
    }

    public Constructor<T>[] getConstructors() {
        // 省略了具体的实现，包括权限检查和构造器填充
        // 实际实现会从方法区获取相关信息
    }

    public Method[] getMethods() {
        // 同样省略了具体的实现，包括权限检查和方法填充
        // 实际实现会遍历方法区的所有方法
    }

    public Field[] getFields() {
        // 省略了具体的实现，包括权限检查和字段填充
        // 实际实现会遍历字段信息
    }

    public Class<? super T> getSuperclass() {
        // 省略了具体的实现，会返回超类的Class对象
        // 实际实现会查询类的继承关系
    }

    protected Constructor<T> getConstructor(Class<?>... parameterTypes)
            throws NoSuchMethodException, SecurityException {
        // 省略了异常处理和查找逻辑
        // 实际实现会根据参数类型查找合适的构造器
    }

    public T newInstance()
            throws InstantiationException, IllegalAccessException {
        // 省略了异常处理和实例化逻辑
        // 实际实现会调用默认构造器创建新实例
    }

    static {
        registerNatives();
    }

    // 省略了其他方法和字段
}
```

### 反射使用

在Java中，Class类与java.lang.reflect类库一起对反射技术进行了全力的支持。

- Constructor类表示的是Class 对象所表示的类的构造方法，利用它可以在运行时动态创建对象
- Field表示Class对象所表示的类的成员变量，通过它可以在运行时动态修改成员变量的属性值(包含private)
- Method表示Class对象所表示的类的成员方法，通过它可以动态调用对象的方法(包含private)

#### Class对象获取

获取Class对象的方法主要有三种：

1. 使用.class语法

   这是最直接也是最常用的方法，对于已知的类型，可以直接使用.class来获取其对应的Class对象。

   ```java
   Class<String> stringClass = String.class;
   ```

2. 使用getClass()方法

   如果你有一个对象实例，可以通过调用这个对象的getClass()方法来获取它的Class对象。

   ```java
   String example = "Hello";
   Class<?> clazz = example.getClass();
   ```

3. 使用Class.forName()方法

   如果你知道类的全限定名（包括包名），可以使用Class.forName()静态方法来获取其Class对象。这个方法在需要动态加载类时非常有用。

   ```java
   try {
       Class<?> clazz = Class.forName("java.lang.String");
   } catch (ClassNotFoundException e) {
       e.printStackTrace();
   }
   ```

**注意事项**

- Class.forName()方法会检查指定的类是否已被加载到JVM中，如果没有，则会尝试加载这个类。如果加载过程中出现错误（例如找不到类定义），它会抛出ClassNotFoundException。
- 使用反射操作时，需要注意权限问题，比如访问私有成员或方法可能需要相应的访问控制。
- 获取到Class对象后，可以进一步进行反射操作，如创建对象实例、获取方法或字段、调用方法等。

#### Constructor类及其用法

Constructor类存在于反射包(java.lang.reflect)中，反映的是Class 对象所表示的类的构造方法，它提供了访问和操作构造器的功能。以下是关于Constructor对象的一些关键点：

1. 获取Constructor对象：
   - 使用Class对象的getConstructors()方法获取所有公共构造器
   - 使用Class对象的getDeclaredConstructors()方法获取类声明的所有构造器，包括私有的和受保护的
   - 使用Class对象的getConstructor(Class<?>... parameterTypes)方法获取具有特定参数类型的公共构造器
   - 使用Class对象的getDeclaredConstructor(Class<?>... parameterTypes)方法获取具有特定参数类型的构造器，不论其访问修饰符
2. 构造器的调用：
   - Constructor对象有一个newInstance(Object... args)方法，用于创建并初始化类的新实例。传递给newInstance的参数必须与构造器的参数类型相匹配
3. 构造器的属性：
   - Constructor对象有自己的isSynthetic()方法，用于检查构造器是否是合成的（即不是由用户代码直接声明的）。
   - Constructor对象的getModifiers()方法返回构造器的修饰符，如public、private等。
   - Constructor对象的getParameterTypes()方法返回构造器参数的类型数组。
   - Constructor对象的getDeclaringClass()方法返回声明此构造器的类。
4. 构造器的权限和异常处理：
   - 反射调用构造器可能会涉及到权限检查，如果调用私有构造器，需要确保有足够的权限。
   - 如果构造器抛出异常，newInstance()方法会封装这些异常并在运行时抛出InvocationTargetException。
5. 构造器的重载：
   - 类可以有多个构造器，每个构造器都有不同的参数列表，这称为构造器的重载。通过getConstructors()或getDeclaredConstructors()可以获取重载的构造器集合。

```java
import java.lang.reflect.Constructor;

public class Test {
    public Test(int a, String b) {}

    public static void main(String[] args) throws Exception {
        Class<?> clazz = Test.class;
        Constructor<?> constructor = clazz.getDeclaredConstructor(int.class, String.class);
        Object instance = constructor.newInstance(123, "example");
    }
}
```

在这个例子中，我们首先获取了Test类的Constructor对象，然后使用它来创建一个新的Test类实例。

#### Field类及其用法

Field类是Java反射API的一部分，它代表了类或接口中的一个字段（属性）。通过Field对象，可以在运行时动态地访问和修改对象的字段值，包括私有字段。以下是Field类的一些基本用法和方法：

1. 获取Field对象：
   - 使用getDeclaredField(String name)方法获取声明在该类中的指定名称的字段，包括私有字段
   - 使用getField(String name)方法获取该类及其父类中公共（public）的指定名称的字段
   - 使用getDeclaredFields()方法获取Class对象所表示的类或接口的所有(包含private修饰的)字段,不包括继承的字段
   - 使用getFields()方法获取修饰符为public的字段，包含继承字段
2. 操作字段值：
   - 使用get(Object obj)方法获取指定对象的此字段的值。如果字段是静态的，可以传递null作为对象参数。
   - 使用set(Object obj, Object value)方法设置指定对象的此字段的值。同样，静态字段可以使用null作为对象参数。
3. 访问修饰符和权限：
   - 使用getModifiers()方法获取字段的修饰符，如public、private等。
   - 在访问私有字段时，可能需要先调用setAccessible(true)来取消Java的访问控制检查。
4. 类型和名称
   - 使用getType()方法获取字段的类型。
   - 使用getName()方法获取字段的名称。

示例：

假设有一个类Person，包含一个私有字段name。

```java
public class Person {
    private String name;

    // 构造函数、getter和setter省略...
}

```

你可以这样使用Field来访问和修改Person类的name字段：

```java
import java.lang.reflect.Field;

public class FieldExample {
    public static void main(String[] args) throws Exception {
        Person person = new Person();
        
        // 获取Person类的Class对象
        Class<?> clazz = Person.class;
        
        // 获取名为"name"的Field对象
        Field nameField = clazz.getDeclaredField("name");
        
        // 允许访问私有字段
        nameField.setAccessible(true);
        
        // 设置字段值
        nameField.set(person, "Alice");
        
        // 获取字段值
        String name = (String) nameField.get(person);
        System.out.println(name);  // 输出: Alice
    }
}

```

**注意事项：**

- 使用反射修改私有字段可能会破坏类的封装性和安全性，应谨慎使用。
- 在使用反射操作之前，确保理解其潜在的风险和性能影响。
- 当访问或修改静态字段时，不需要提供具体的对象实例。

#### Method类及其用法

Method类是Java反射API中的一个核心类，它代表了类或接口中的一个方法。通过Method对象，可以在运行时动态地获取和操作类的方法信息，包括调用方法、获取方法的参数类型和返回类型等。以下是Method类的一些基本用法和方法：

1. 获取Method对象
   - 使用getMethod(String name, Class<?>... parameterTypes)方法获取指定的公共方法，包括继承的方法。
   - 使用getDeclaredMethod(String name, Class<?>... parameterTypes)方法获取声明在此类中的指定方法，不考虑继承，可以获取私有方法。
   - 使用getMethods()方法获取一个包含某些 Method 对象的数组，这些对象反映此 Class 对象所表示的类或接口（包括那些由该类或接口声明的以及从超类和超接口继承的那些的类或接口）的公共 member 方法。
   - 使用getDeclaredMethods()方法获取一个包含某些 Method 对象的数组，这些对象反映此 Class 对象表示的类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。
2. 调用方法
   - 使用invoke(Object obj, Object... args)方法动态地调用对象的某个方法。obj是方法所属对象的实例（如果是静态方法，可以传入null），args是方法调用时的参数列表。
3. 方法信息
   - 名称与返回类型：使用getName()获取方法名，使用getReturnType()获取返回类型。
   - 参数信息：使用getParameterTypes()获取参数类型数组，使用getParameterCount()获取参数数量。
   - 异常信息：使用getExceptionTypes()获取方法声明可能抛出的异常类型数组。
   - 访问修饰符：使用getModifiers()获取方法的访问修饰符，如public、private等。
   - 注解信息：使用getAnnotations()获取方法上的注解数组。

示例：

假设有一个类Calculator，包含一个加法方法add。

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

你可以这样使用Method来调用Calculator类的add方法：

```java
import java.lang.reflect.Method;

public class MethodExample {
    public static void main(String[] args) throws Exception {
        Calculator calculator = new Calculator();
        
        // 获取Calculator类的Class对象
        Class<?> clazz = Calculator.class;
        
        // 获取名为"add"的Method对象
        Method addMethod = clazz.getMethod("add", int.class, int.class);
        
        // 调用方法并传递参数
        int result = (int) addMethod.invoke(calculator, 5, 3);
        System.out.println(result);  // 输出: 8
    }
}
```

**注意事项：**

- 调用私有方法或受保护方法时，需要先调用setAccessible(true)来忽略Java的访问控制检查。
- invoke方法可能会抛出IllegalAccessException、IllegalArgumentException、InvocationTargetException等异常，需要妥善处理。
- 反射调用方法相比直接调用有性能开销，应避免在性能敏感的代码中频繁使用。

## 泛型

Java泛型是Java语言从JDK 1.5版本开始引入的一个重要特性，旨在提高代码的重用性、类型安全性和可读性。

- 泛型（Generics） 允许你在类、接口和方法中使用类型参数，从而实现参数化类型。这意味着你可以创建一个能够适用于多种数据类型的类或方法，而无需知道实际类型是什么，直到实例化时确定。
- 泛型的本质是参数化类型（Parameterized Type），即类型本身可以作为参数传递。
- 引入泛型后，编译器可以在编译时期检查类型安全，避免了强制类型转换的错误和运行时的ClassCastException。

### 泛型的使用

#### 泛型类

泛型类是Java中使用泛型来定义的一种类，它允许类在声明时包含一个或多个类型参数，这些参数代表了类中某些成员变量或方法的类型。这样，当创建泛型类的实例时，可以指定具体的类型，从而确保代码的类型安全性和减少类型转换。

1. 定义泛型类

   - 单元泛型

     泛型类的定义通常包含类型参数列表，放在类名的尖括号内。例如，我们可以定义一个名为Box的泛型类，它有一个类型参数T：

     ```java
     public class Box<T> {
         private T item;
         
         public void set(T item){
             this.item = item;
         }
         
         public T get(){
             return item;
         }
     }
     ```

     在这个例子中，T是一个类型参数，代表某种未知的类型。在类的实例化过程中，T可以被任何引用类型替换，如String、Integer等。

   - 多元泛型

     ```java
     public class Notepad<K,V>{       
         private K key ;     
         private V value ;   
         public K getKey(){  
             return this.key ;  
         }  
         public V getValue(){  
             return this.value ;  
         }  
         public void setKey(K key){  
             this.key = key ;  
         }  
         public void setValue(V value){  
             this.value = value ;  
         }  
     } 
     ```

2. 类型参数的使用

   在泛型类的内部，类型参数可以用于声明成员变量、构造函数参数、方法参数和返回类型。例如，Box类的item成员变量和set、get方法都使用了类型参数T：

   - private T item;：成员变量item的类型由T决定。
   - public void set(T item)：set方法接受一个T类型的参数。
   - public T get()：get方法返回一个T类型的结果。

3. 创建泛型类的实例

   当我们创建泛型类的实例时，需要提供具体的类型来替换类型参数。例如，创建一个存储String的Box：

   ```java
   Box<String> stringBox = new Box<>();
   stringBox.set("Hello, World!");
   String value = stringBox.get(); // "Hello, World!"
   ```

   在这个例子中，T被替换为String，因此set和get方法处理的是字符串。

4. 边界约束

   有时候，我们希望类型参数遵循一定的限制，比如只能是某个类或接口的子类。这时，我们可以使用extends关键字来设置上界。例如，如果我们想要一个只允许Number或其子类的Box：

   ```java
   public class Box<T extends Number> {
       // ...
   }
   ```

   现在，Box只能存储Number或其子类的对象，如Integer、Double等。

5. 无界通配符

   在某些情况下，我们可能不需要知道确切的类型，只需要知道它是某种类型或其子类型即可。这时可以使用无界通配符?，例如：

   ```java
   public void printBox(Box<?> box) {
       System.out.println(box.get());
   }
   ```

   这里的<?>表示Box可以是任何类型，但你只能从中获取元素，不能设置新的元素。

6. 类型擦除

   需要注意的是，Java的泛型是通过类型擦除来实现的，这意味着在运行时，泛型信息会被删除。因此，泛型类的实例在运行时并不知道它们的具体类型，所有的类型检查都在编译时完成。
   泛型类在实际编程中非常有用，尤其是在处理集合类和容器类时，可以确保集合中的元素都是同一种类型，提高代码的类型安全性和可读性。

#### 泛型接口

泛型接口是Java中接口定义的一种形式，它允许接口中包含类型参数。这使得实现该接口的类或扩展该接口的其他接口能够在实现或继承时指定具体类型。泛型接口提高了代码的灵活性和重用性，同时也保证了类型安全。以下是关于泛型接口的详细说明：

1. 定义泛型接口

   泛型接口的定义方式类似于泛型类，即在接口名称后面加上尖括号包裹的类型参数列表。例如，定义一个简单的泛型接口Comparable：

   ```java
   public interface Comparable<T> {
   	int compareTo(T other);
   }
   ```

   在这个例子中，T是一个类型参数，表示待比较的类型。实现这个接口的类需要指定T的具体类型，以便compareTo方法可以比较相同类型的对象。

2. 实现泛型接口

   实现泛型接口的类需要在类声明时指定类型参数。以实现上述Comparable接口为例：

   ```java
   public class Person implements Comparable<Person> {
       private String name;
       
       // 构造方法、getter和setter省略
       
       @Override
       public int compareTo(Person other) {
           return this.name.compareTo(other.name);
       }
   }
   ```

   这里，Person类实现了Comparable<Person>，意味着compareTo方法将用于比较两个Person对象的name属性。

3. 多个参数类型

   泛型接口也可以定义多个类型参数，例如：

   ```java
   public interface Pair<K, V> {
       K getKey();
       V getValue();
   }
   ```

   实现此类接口时，需要同时指定两个类型参数：

   ```java
   public class MyPair<K, V> implements Pair<K, V> {
       private K key;
       private V value;
       
       // 构造方法、getter和setter省略
       
       @Override
       public K getKey() {
           return key;
       }
       
       @Override
       public V getValue() {
           return value;
       }
   }
   ```

4. 泛型接口的边界约束

   与泛型类类似，泛型接口也可以定义类型参数的上界或下界，以限制实现该接口的类或接口的类型。例如，定义一个接口要求其实现类必须是Number的子类：

   ```java
   public interface NumberComparator<T extends Number> {
       int compare(T a, T b);
   }
   ```

5. 总结

   泛型接口是Java泛型机制的重要组成部分，它允许接口定义更加灵活，增加了代码的通用性和重用性。通过在接口中使用类型参数，可以确保实现该接口的类或方法能够处理多种类型的数据，同时保持类型安全。理解泛型接口的定义、实现以及如何利用边界约束，对于编写高效、灵活的Java代码至关重要。

#### 泛型方法

泛型方法是Java泛型的一个重要特性，它允许在方法的定义中引入自己的类型参数，这意味着方法可以独立于其所属类是否为泛型类，而拥有泛型能力。泛型方法可以增强代码的灵活性和重用性，使得方法能够适用于多种数据类型。以下是关于泛型方法的详细说明：

1. 定义泛型方法

   泛型方法的定义是在方法的返回类型之前放置类型参数列表，用尖括号包围。例如，定义一个交换两个元素位置的泛型方法：

   ```java
   public class Utility{
       // 泛型方法 swap，适用于任何类型T
       public static <T> void swap(T[] array, int i, int j){
           T temp = array[i];
           array[i] = array[j];
           array[j] = temp;
       }
   }
   ```

   在这个例子中，<T>声明了一个类型参数T，它被用于方法的参数array及其中的元素类型。这意味着swap方法可以用于交换任何引用类型的数组元素，如Integer[]、String[]等。

2. 调用泛型方法

   调用泛型方法时，编译器会根据实际传递的参数类型自动推断类型参数，你通常不需要显式指定类型参数。例如：

   ```java
   Integer[] numbers = {1, 2, 3, 4};
   Utility.swap(numbers, 0, 1); // 自动推断T为Integer
   ```

3. 显式指定类型参数

   在某些情况下，可能需要显式指定泛型方法的类型参数，尤其是在编译器无法准确推断类型时：

   ```java
   Utility.<String>swap(new String[]{"a", "b"}, 0, 1); // 显式指定T为String
   ```

4. 泛型方法与泛型类的关系

   - 独立性：泛型方法可以在非泛型类中定义，也可以在泛型类中定义。即使所在类是泛型类，泛型方法也可以有自己独立的类型参数，与类的类型参数无关。
   - 兼容性：泛型方法也可以使用其所在类的类型参数，但这样做不是必须的。

5. 泛型方法的边界约束

   泛型方法同样支持边界约束，可以使用extends关键字指定类型参数的上界，或者使用super指定下界。例如，限制类型参数必须是Comparable的子类：

   ```java
   public static <T extends Comparable<T>> int compare(T a, T b) {
       return a.compareTo(b);
   }
   ```

6. 总结

   泛型方法是Java泛型机制灵活运用的体现，它使得方法能够以类型安全的方式处理多种数据类型，提高了代码的通用性和重用性。通过在方法级别引入类型参数，开发者可以设计出更加灵活和强大的API，满足不同场景下的需求。

## 集合

Java集合是Java语言中提供的一种数据结构框架，用于存储和操作对象的容器。集合框架位于java.util包中，它是一个统一的架构，允许我们对各种数据结构（如列表、集合、映射等）进行操作。Java集合框架主要包括以下几类：

1. **Collection接口**：这是集合框架的根接口，一个独立的元素序列。其直接子接口包括List、Set。
   - **List接口**：表示一个有序的集合，允许元素重复，提供了按索引访问元素的方法。常用的实现类有ArrayList、LinkedList和Vector。
   - **Set接口**：不允许包含重复元素的集合，不保证元素的顺序。主要实现类有HashSet（无序，基于哈希表）、LinkedHashSet（有序，基于哈希表和双向链表）和TreeSet（有序，基于红黑树）。
2. **Map接口**：不同于Collection，Map是一种键值对(key-value pair)的存储结构。每个元素包含一个键对象和一个值对象，键是唯一的。主要实现类有HashMap（非线程安全，基于哈希表）、LinkedHashMap（保持插入顺序或访问顺序）、TreeMap（按键排序，基于红黑树）和ConcurrentHashMap（线程安全，高效并发访问）。
3. **迭代器(Iterator)与列表迭代器(ListIterator)**：用于遍历集合中的元素。Iterator只能向前遍历，而ListIterator在List上使用时，还可以向后遍历以及修改元素。
4. **流(Stream API)**：Java 8引入了Stream API，它提供了一种新的处理集合数据的方式，可以以声明性方式处理数据集合，如过滤、映射、归约等操作，更加方便高效。
5. **工具类**：如Collections，提供了对集合进行排序、搜索、转换等各种操作的静态方法。

Java集合框架的设计遵循了泛型，这意味着你可以在创建集合时指定它们将持有的对象类型，从而增加了类型安全性和编译时检查。此外，集合框架还支持线程安全和非线程安全的实现，开发者可以根据具体需求选择合适的集合类。

### ArrayList源码解析

#### **概述**

`ArrayList`是Java集合框架中一个非常常用且功能强大的实现类，它继承自`List`接口。`ArrayList`内部是通过数组来实现的，这意味着它能够提供快速的随机访问（通过索引访问元素），但相较于链表结构，在进行插入和删除操作时可能效率较低，尤其是在列表的中间位置进行这些操作时。

`ArrayList`的底层实现主要依赖于一个可动态调整大小的Object数组。以下是几个关键点和部分源码概述：

1. 默认初始化容量：

   `ArrayList`在没有指定初始容量时，默认创建一个容量为10的Object数组来存储元素。这个默认容量定义为`private static final int DEFAULT_CAPACITY = 10;`。

2. 构造方法：

   - 无参构造函数：public `ArrayList()`，会初始化一个默认容量的数组。
   - 指定容量构造函数：`public ArrayList(int initialCapacity)`，用户可以指定初始容量，避免了多次自动扩容的开销。
   - 复制构造函数：还可以通过一个集合实例来创建ArrayList，例如`ArrayList(Collection<? extends E> c)`，这将创建一个包含指定集合元素的新ArrayList。

3. 动态扩容机制：

   当数组空间不足时，`ArrayList`会自动扩容。扩容通常是原容量的1.5倍，通过计算得到新容量：`newCapacity = oldCapacity + (oldCapacity >> 1)`。扩容操作涉及创建一个更大的数组，并将原数组的所有元素复制到新数组中。

4. 添加元素：

   添加元素时，如果数组已满，则先执行扩容操作，然后将元素放入数组的相应位置。例如，`public boolean add(E e)`方法实现这一逻辑。

5. 删除元素：

   删除操作如`public E remove(int index)`，会移动索引大于被删除元素位置的所有后续元素，以填补空位，并返回被删除的元素。

6. 快速访问：

   由于使用数组实现，通过索引访问元素(`public E get(int index)`)非常快速。

7. 序列化与扩容策略：

   `ArrayList`实现了`Serializable`接口，支持序列化。它内部有几种不同的空数组常量，用于优化序列化时的空间占用，例如`EMPTY_ELEMENTDATA`、`DEFAULTCAPACITY_EMPTY_ELEMENTDATA`等，分别用于不同情况下的实例化。

#### **底层数据结构**

```java
    /**
     * 默认初始容量
     */
    private static final int DEFAULT_CAPACITY = 10;

    /**
     * 用于创建空对象的共享空数组实例
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};

    /**
     * 用于默认大小的空数组实例的共享空数组实例。我们区分这个与EMPTY_ELEMENTDATA，以便在添加第一个元素时知道要膨胀多少
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};

    /**
     * ArrayList 中元素所存储的数组缓冲区
     * ArrayList 的容量是该数组缓冲区的长度
     * 任何空的 ArrayList（elementData 等于 DEFAULTCAPACITY_EMPTY_ELEMENTDATA）在添加第一个元素时都会被扩展为 			 * DEFAULT_CAPACITY
     */
    transient Object[] elementData; // 非私有，以简化嵌套类访问

    /**
     * ArrayList的大小（它包含的元素数量）。
     *
     * @serial
     */
    private int size;
```

#### **构造函数**

```java
    /**
     * 构造一个具有指定初始容量的空列表
     *
     * @param  initialCapacity  列表的初始容量
     * @throws IllegalArgumentException 如果指定的初始容量为负数
     */
    public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }

    /**
     * 构建一个初始容量为10的空列表
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }

    /**
     * 根据指定集合的迭代器返回的顺序，构建一个包含该集合元素的列表。
     *
     * @param c 要放入此列表中的元素的集合
     * @throws 如果指定的集合为null，则抛出NullPointerException
     */
    public ArrayList(Collection<? extends E> c) {
        elementData = c.toArray();
        if ((size = elementData.length) != 0) {
            // c.toArray 可能（错误地）不返回 Object[] 类型的数组
            if (elementData.getClass() != Object[].class)
                elementData = Arrays.copyOf(elementData, size, Object[].class);
        } else {
            // 替换为空数组
            this.elementData = EMPTY_ELEMENTDATA;
        }
    }
```

#### **自动扩容**

`ArrayList`的自动扩容机制是为了在集合中添加元素时，如果当前数组容量不足以存放新元素，自动增加数组容量的过程。下面是该机制的具体实现逻辑：

1. 初始化与默认容量：
   - 当使用无参构造器创建`ArrayList`时，它并不会立即分配实际的数组空间，而是初始化一个空的引用`elementData`。首次添加元素时，如果数组尚未分配，则会分配一个默认容量的数组，这个默认容量通常是10。
   - 也可以在构造`ArrayList`时通过参数指定初始容量。
2. 添加元素时的容量检查：
   - 在调用`add(E e)`方法添加元素时，`ArrayList`会检查当前数组`elementData`的长度是否能容纳下一个元素。
   - 如果当前数组已满（即元素数量等于数组长度），则触发扩容机制。
3. 计算新容量：
   - 扩容的核心方法是`grow(int minCapacity)`，其中`minCapacity`是至少需要的容量。
   - 计算新容量的基本逻辑是将旧容量乘以1.5（即`oldCapacity + (oldCapacity >> 1)`），这是一种快速的乘法表示方法，相当于将容量扩大1.5倍，以此来减少频繁扩容导致的性能开销。
   - 如果计算出的新容量仍小于`minCapacity`（即需要的最小容量），则新容量会被设置为`minCapacity`。
4. 数组复制与更新引用：
   - 使用`Arrays.copyOf()`方法创建一个新的数组，长度为计算得到的新容量。
   - 将原数组`elementData`中的所有元素复制到新数组中。
   - 更新`ArrayList`内部的`elementData`引用，使其指向新数组。
   - 在新数组中添加新元素，并更新元素数量`size`。
5. 容量上限：
   - `ArrayList`的最大容量受`Integer.MAX_VALUE`限制，即大约为2^31 - 1。当数组接近这个最大值时，再次扩容可能只会增加一个元素的空间，因为超过这个界限会导致索引超出整型范围。

通过上述步骤，`ArrayList`实现了动态地调整其内部数组的大小，以适应不断添加的元素，同时尽量减少了扩容操作带来的性能开销。

```java
    /**
     * 如果必要，增加此<tt>ArrayList</tt>实例的容量，以确保它能容纳至少由最小容量参数指定的元素数量。
     *
     * @param   minCapacity   期望的最小容量
     */
    public void ensureCapacity(int minCapacity) {
        int minExpand = (elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA)
            // 如果不是默认元素表，则为任何尺寸
            ? 0
            // 大于默认值的默认空表。它已经被设定为默认大小
            : DEFAULT_CAPACITY;

        if (minCapacity > minExpand) {
            ensureExplicitCapacity(minCapacity);
        }
    }

    private static int calculateCapacity(Object[] elementData, int minCapacity) {
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
    }

    private void ensureCapacityInternal(int minCapacity) {
        ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
    }

    private void ensureExplicitCapacity(int minCapacity) {
        modCount++;

        // 注重防溢出的代码
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }

    /**
     * 分配数组的最大大小
     * 一些虚拟机在数组中预留了一些头信息字
     * 尝试分配更大的数组可能会导致内存溢出错误：请求的数组大小超过了VM限制。
     */
    private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

    /**
     * 增加容量以确保它能够容纳至少由最小容量参数指定的数量的元素
     *
     * @param minCapacity 期望的最小容量
     */
    private void grow(int minCapacity) {
        // 注重防溢出的代码
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity 通常接近于 size，因此这是一个有利因素：
        elementData = Arrays.copyOf(elementData, newCapacity);
    }

    private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // 溢出
            throw new OutOfMemoryError();
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

![ArrayList_grow](https://pdai.tech/images/collection/ArrayList_grow.png)

#### **add()，addAll()**

```java
	/**
     * 将指定的元素追加到此列表的末尾。
     *
     * @param e 要添加到此列表中的元素
     * @return <tt>true</tt>
     */
    public boolean add(E e) {
        ensureCapacityInternal(size + 1);  // 增加 modCount!!
        elementData[size++] = e;
        return true;
    }

    /**
     * 在列表的指定位置插入指定的元素。将当前位于该位置的元素（如果有的话）及其后的所有元素向右移动（它们的索引+1）。
     *
     * @param index 指定元素要插入的索引位置
     * @param element 要插入的元素
     * @throws IndexOutOfBoundsException
     */
    public void add(int index, E element) {
        rangeCheckForAdd(index);

        ensureCapacityInternal(size + 1);  // 增加 modCount!!
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
        elementData[index] = element;
        size++;
    }
```

![ArrayList_add](https://pdai.tech/images/collection/ArrayList_add.png)

```java
	/**
     * 将指定集合中的所有元素按照该集合的迭代器返回的顺序追加到此列表的末尾
     * 这个操作的行为是未定义的，如果在操作进行过程中指定的集合被修改了。
     * （这意味着，如果指定的集合就是这个列表，并且这个列表非空，那么这个调用的行为也是未定义的。）
     *
     * @param c 包含要添加到此列表中的元素的集合
     * @return <tt>true</tt> 如果这个列表因为调用而发生了变化
     * @throws NullPointerException 如果指定的集合为null
     */
    public boolean addAll(Collection<? extends E> c) {
        Object[] a = c.toArray();
        int numNew = a.length;
        ensureCapacityInternal(size + numNew);  // 增加 modCount
        System.arraycopy(a, 0, elementData, size, numNew);
        size += numNew;
        return numNew != 0;
    }

    /**
     * 插入指定集合中的所有元素到此列表中，从指定位置开始。
     * 将位于该位置的元素（如果有的话）及其后面的所有元素向右移动（增加它们的索引）。
     * 新元素将按照指定集合的迭代器返回的顺序出现在列表中。
     *
     * @param index 要插入来自指定集合的第一个元素的索引位置
     * @param c 包含要添加到此列表中的元素的集合
     * @return <tt>true</tt> 如果这个列表因为调用而发生了变化
     * @throws IndexOutOfBoundsException {@inheritDoc}
     * @throws NullPointerException 如果指定的集合为null
     */
    public boolean addAll(int index, Collection<? extends E> c) {
        rangeCheckForAdd(index);

        Object[] a = c.toArray();
        int numNew = a.length;
        ensureCapacityInternal(size + numNew);  // 增加 modCount

        int numMoved = size - index;
        if (numMoved > 0)
            System.arraycopy(elementData, index, elementData, index + numNew,
                             numMoved);

        System.arraycopy(a, 0, elementData, index, numNew);
        size += numNew;
        return numNew != 0;
    }
```

#### **set()**

```java
    /**
     * 将此列表中指定位置的元素替换为指定的元素
     *
     * @param index 要替换的元素的索引
     * @param element 要存储在指定位置的元素
     * @return 先前位于指定位置的元素
     * @throws IndexOutOfBoundsException
     */
    public E set(int index, E element) {
        rangeCheck(index);

        E oldValue = elementData(index);
        elementData[index] = element;
        return oldValue;
    }
```

#### **get()**

```java
    /**
     * 返回此列表中指定位置的元素
     *
     * @param  index 要返回的元素的索引
     * @return 此列表中指定位置的元素
     * @throws IndexOutOfBoundsException
     */
    public E get(int index) {
        rangeCheck(index);

        return elementData(index);
    }
```

#### **remove()**

```java
    /**
     * 移除此列表中指定位置的元素。
     * 将所有后续元素向左移动（从它们的索引中减去一）。
     *
     * @param 要移除的元素的索引
     * @return 从列表中移除的元素
     * @throws IndexOutOfBoundsException
     */
    public E remove(int index) {
        rangeCheck(index);

        modCount++;
        E oldValue = elementData(index);

        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
            elementData[--size] = null; // 让垃圾回收机制能够正常运行

        return oldValue;
    }

    /**
     * 从该列表中移除指定元素的第一个出现，如果存在的话
     * 如果列表中不包含该元素，则列表保持不变
     *
     * @param o 要从该列表中移除的元素，如果存在的话
     * @return <tt>true</tt> 如果这个列表包含了指定的元素
     */
    public boolean remove(Object o) {
        if (o == null) {
            for (int index = 0; index < size; index++)
                if (elementData[index] == null) {
                    fastRemove(index);
                    return true;
                }
        } else {
            for (int index = 0; index < size; index++)
                if (o.equals(elementData[index])) {
                    fastRemove(index);
                    return true;
                }
        }
        return false;
    }
```

#### **trimToSize()**

```java
    /**
     * 此操作将此 <tt>ArrayList</tt> 实例的容量缩小为列表当前的大小。
     * 应用程序可以使用此操作来最小化 <tt>ArrayList</tt> 实例的存储需求。
     */
    public void trimToSize() {
        modCount++;
        if (size < elementData.length) {
            elementData = (size == 0)
              ? EMPTY_ELEMENTDATA
              : Arrays.copyOf(elementData, size);
        }
    }
```

#### **indexOf()，lastIndexOf()**

```java
    /**
     * 返回指定元素在该列表中第一次出现的索引，如果该列表不包含此元素，则返回-1
     */
    public int indexOf(Object o) {
        if (o == null) {
            for (int i = 0; i < size; i++)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = 0; i < size; i++)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }

    /**
     * 返回指定元素在该列表中最后一次出现的索引，如果该列表不包含此元素，则返回-1。
     */
    public int lastIndexOf(Object o) {
        if (o == null) {
            for (int i = size-1; i >= 0; i--)
                if (elementData[i]==null)
                    return i;
        } else {
            for (int i = size-1; i >= 0; i--)
                if (o.equals(elementData[i]))
                    return i;
        }
        return -1;
    }
```

#### **Fail Fast机制**

在Java中，ArrayList的“Fail-Fast”机制是一种错误快速检测的行为，主要应用于迭代器（Iterator）和列表迭代器（ListIterator）中。当多个线程同时访问集合对象，且其中一个线程修改了集合的结构（比如添加或删除元素），而其他线程正在进行迭代时，Fail-Fast机制会抛出`ConcurrentModificationException`异常，以迅速反馈并发修改的问题，防止产生不可预期的结果。

Fail-Fast机制是通过一个名为`modCount`的变量来实现的。每当对ArrayList进行添加、删除等修改结构的操作时，`modCount`的值就会增加。迭代器在迭代过程中会保留一个名为`expectedModCount`的变量，其值初始化为集合的`modCount`值。每次迭代前，迭代器都会检查`modCount`是否仍然等于`expectedModCount`，如果不等，则说明集合已经被其他线程修改过，此时迭代器就会抛出`ConcurrentModificationException`异常。

需要注意的是，Fail-Fast机制并不能保证绝对的线程安全，它只是提供了一种检测并发修改的手段。如果需要在多线程环境下安全地使用ArrayList，应该使用`Collections.synchronizedList(List<T> list)`方法将其转换为线程安全的列表，或者使用`CopyOnWriteArrayList`，它是线程安全的，并且在修改时会创建集合的副本，从而避免并发问题，但请注意这会增加内存消耗和一定的性能开销。

### LinkedList源码解析

#### 概述

LinkedList 是 Java 集合框架中一个重要的数据结构实现，它位于 `java.util` 包下，直接实现了 `List` 接口，同时也实现了 `Deque`（双端队列）接口。LinkedList 的核心特点是它基于双向链表实现，这使得它在插入、删除操作上表现得非常高效，尤其是在链表的头部或尾部进行操作时，时间复杂度可以达到 O(1)。然而，与 ArrayList 相比，LinkedList 的随机访问（如按索引查询元素）效率较低，因为这可能需要遍历链表，时间复杂度为 O(n)。

**基本特性：**

1. 双向链表结构：每个节点（Node 或 Entry）除了存储数据外，还持有对其前一个节点和后一个节点的引用，这使得双向遍历成为可能。
2. 动态大小：LinkedList 的大小是动态变化的，可以根据需要自动调整。
3. 功能丰富：作为 List 接口的实现，LinkedList 支持所有列表操作，如添加（`add()`）、删除（`remove()`）、修改（`set()`）和访问（`get()`）元素。同时，由于实现了 Deque 接口，LinkedList 还提供了在首尾两端高效添加和移除元素的方法，如 `addFirst()`、`addLast()`、`removeFirst()`、`removeLast()` 等。
4. 队列和栈操作：可以当作堆栈（LIFO，后进先出）或队列（FIFO，先进先出）使用，因为提供了相应的方法。
5. 非同步（非线程安全）：默认情况下，LinkedList 不是线程安全的。如果需要在多线程环境下使用，可以通过 `Collections.synchronizedList()` 方法将其转换为线程安全的列表。
6. 允许null值：LinkedList 允许存储 `null` 元素。

**关键属性：**

- `transient Node<E> first;`：指向链表的第一个节点。
- `transient Node<E> last;`：指向链表的最后一个节点。
- `transient int size;`：维护链表中元素的数量。

![LinkedList_base](https://pdai.tech/images/collection/LinkedList_base.png)

#### **底层数据结构**

```java
    transient int size = 0;

    /**
     * 指向头节点的指针
     * 约束条件: (first == null && last == null) ||
     *            (first.prev == null && first.item != null)
     */
    transient Node<E> first;

    /**
     * 指向尾节点的指针
     * 约束条件: (first == null && last == null) ||
     *            (last.next == null && last.item != null)
     */
    transient Node<E> last;
```

其中，Node是私有的静态内部类，这是为了设计上的清晰和逻辑上的独立性，每个Node节点都独立存在。

```java
    private static class Node<E> {
        E item;
        Node<E> next;
        Node<E> prev;

        Node(Node<E> prev, E element, Node<E> next) {
            this.item = element;
            this.next = next;
            this.prev = prev;
        }
    }
```

#### **构造函数**

```java
    /**
     * 构建一个空的list
     */
    public LinkedList() {
    }

    /**
     * 根据指定集合中元素的返回顺序，构建一个包含这些元素的列表
     *
     * @param c 要放入此列表中的元素的集合
     * @throws NullPointerException 如果指定列表为空
     */
    public LinkedList(Collection<? extends E> c) {
        this();
        addAll(c);
    }
```

#### **getFirst()，getLast()**

获取第一个元素，获取最后一个元素：

```java
    /**
     * 返回列表的第一个元素
     *
     * @return 列表的第一个元素
     * @throws NoSuchElementException 如果列表为空
     */
    public E getFirst() {
        final Node<E> f = first;
        if (f == null)
            throw new NoSuchElementException();
        return f.item;
    }

    /**
     * 返回列表的最后一个元素
     *
     * @return 列表的最后一个元素
     * @throws NoSuchElementException 如果列表为空
     */
    public E getLast() {
        final Node<E> l = last;
        if (l == null)
            throw new NoSuchElementException();
        return l.item;
    }
```

#### **removeFirst()，removeLast()，remove(Object o)，remove(int index)**

`remove()`方法也有两个版本，一个是删除跟指定元素相等的第一个元素`remove(Object o)`，另一个是删除指定下标处的元素`remove(int index)`。

![LinkedList_remove.png](https://pdai.tech/images/collection/LinkedList_remove.png)

remove(Object o)：

```java
    /**
     * 从该列表中移除指定元素的第一个出现，如果存在的话。
     * 如果此列表不包含该元素，则保持不变。
     *
     * @param o 要从该列表中移除的元素，如果存在的话
     * @return {@code true} 如果这个列表包含了指定的元素
     */
    public boolean remove(Object o) {
        if (o == null) {
            for (Node<E> x = first; x != null; x = x.next) {
                if (x.item == null) {
                    unlink(x);
                    return true;
                }
            }
        } else {
            for (Node<E> x = first; x != null; x = x.next) {
                if (o.equals(x.item)) {
                    unlink(x);
                    return true;
                }
            }
        }
        return false;
    }

    E unlink(Node<E> x) {
        // 断言 x != null;
        final E element = x.item;
        final Node<E> next = x.next;
        final Node<E> prev = x.prev;

        if (prev == null) {
            first = next;
        } else {
            prev.next = next;
            x.prev = null;
        }

        if (next == null) {
            last = prev;
        } else {
            next.prev = prev;
            x.next = null;
        }

        x.item = null;
        size--;
        modCount++;
        return element;
    }
```

remove(int index)：

```java
    /**
     * 移除此列表中指定位置的元素。  
     * 将所有后续元素向左移动（从它们的索引中减去一）。
     * 返回从列表中移除的元素。
     *
     * @param index 要移除的元素的索引
     * @return 先前位于指定位置的元素
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public E remove(int index) {
        checkElementIndex(index);
        return unlink(node(index));
    }
```

removeFirst()：

```java
    /**
     * 从这个列表中移除并返回第一个元素。
     *
     * @return 这个列表的第一个元素
     * @throws NoSuchElementException 如果列表为空
     */
    public E removeFirst() {
        final Node<E> f = first;
        if (f == null)
            throw new NoSuchElementException();
        return unlinkFirst(f);
    }

    /**
     * 删除非空的头节点f。
     */
    private E unlinkFirst(Node<E> f) {
        // 断言 f == first && f != null;
        final E element = f.item;
        final Node<E> next = f.next;
        f.item = null;
        f.next = null; // help GC
        first = next;
        if (next == null)
            last = null;
        else
            next.prev = null;
        size--;
        modCount++;
        return element;
    }
```

removeLast()：

```java
    /**
     * 从这个列表中移除并返回最后一个元素。
     *
     * @return 该列表最后一个元素
     * @throws NoSuchElementException 如果列表为空
     */
    public E removeLast() {
        final Node<E> l = last;
        if (l == null)
            throw new NoSuchElementException();
        return unlinkLast(l);
    }

    /**
     * 删除非空的尾节点l。
     */
    private E unlinkLast(Node<E> l) {
        // 断言 l == last && l != null;
        final E element = l.item;
        final Node<E> prev = l.prev;
        l.item = null;
        l.prev = null; // help GC
        last = prev;
        if (prev == null)
            first = null;
        else
            prev.next = null;
        size--;
        modCount++;
        return element;
    }
```

#### add(E e)，add(int index, E element)

![LinkedList_add](https://pdai.tech/images/collection/LinkedList_add.png)

add(E e)：

```java
    /**
     * 将指定的元素追加到此列表的末尾。
     *
     * <p>这种方法等同于 {@link #addLast}.
     *
     * @param e 要添加到此列表中的元素
     * @return {@code true} (如指定的 {@link Collection#add})
     */
    public boolean add(E e) {
        linkLast(e);
        return true;
    }

    /**
     * 将e作为最后一个元素链接
     */
    void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null)
            first = newNode;
        else
            l.next = newNode;
        size++;
        modCount++;
    }
```

add(int index,E element)：

```java
    /**
     * 在列表的指定位置插入指定的元素。
     * 将当前位于该位置的元素（如果有的话）及之后的所有元素向右移动（它们的索引加一）。
     *
     * @param index 指定元素要插入的索引位置。
     * @param element 要插入的元素
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public void add(int index, E element) {
        checkPositionIndex(index);

        if (index == size)
            linkLast(element);
        else
            linkBefore(element, node(index));
    }

    /**
     * 将e作为最后一个元素链接
     */
    void linkLast(E e) {
        final Node<E> l = last;
        final Node<E> newNode = new Node<>(l, e, null);
        last = newNode;
        if (l == null)
            first = newNode;
        else
            l.next = newNode;
        size++;
        modCount++;
    }

    /**
     * 在非空节点 succ 之前插入元素 e。
     */
    void linkBefore(E e, Node<E> succ) {
        // 断言 succ != null;
        final Node<E> pred = succ.prev;
        final Node<E> newNode = new Node<>(pred, e, succ);
        succ.prev = newNode;
        if (pred == null)
            first = newNode;
        else
            pred.next = newNode;
        size++;
        modCount++;
    }

    /**
     * 返回指定元素索引处的（非空）节点。
     */
    Node<E> node(int index) {
        // 断言 isElementIndex(index);

        if (index < (size >> 1)) {
            Node<E> x = first;
            for (int i = 0; i < index; i++)
                x = x.next;
            return x;
        } else {
            Node<E> x = last;
            for (int i = size - 1; i > index; i--)
                x = x.prev;
            return x;
        }
    }
```

#### addAll()

```java
    /**
     * 将指定集合中的所有元素按照该集合的迭代器返回的顺序追加到此列表的末尾。
     * 如果在操作进行过程中修改指定的集合，则此操作的行为是未定义的。
     * （注意，如果指定的集合是这个列表，并且它非空，就会发生这种情况。）
     *
     * @param c 包含要添加到此列表中的元素的集合
     * @return {@code true} 如果这个列表因为调用而发生了变化
     * @throws NullPointerException 如果指定集合为空
     */
    public boolean addAll(Collection<? extends E> c) {
        return addAll(size, c);
    }

    /**
     * 插入指定集合中的所有元素到此列表中，从指定位置开始。
     * 移动当前位于该位置的元素（如果有的话）以及所有后续元素向右（增加它们的索引）。
     * 新元素将按照指定集合的迭代器返回的顺序出现在列表中。
     *
     * @param index 要插入来自指定集合的第一个元素的索引位置
     * @param c 包含要添加到此列表中的元素的集合。
     * @return {@code true} 如果这个列表因为调用而发生了变化
     * @throws IndexOutOfBoundsException {@inheritDoc}
     * @throws NullPointerException 如果指定集合为空
     */
    public boolean addAll(int index, Collection<? extends E> c) {
        checkPositionIndex(index);

        Object[] a = c.toArray();
        int numNew = a.length;
        if (numNew == 0)
            return false;

        Node<E> pred, succ;
        if (index == size) {
            succ = null;
            pred = last;
        } else {
            succ = node(index);
            pred = succ.prev;
        }

        for (Object o : a) {
            @SuppressWarnings("unchecked") E e = (E) o;
            Node<E> newNode = new Node<>(pred, e, null);
            if (pred == null)
                first = newNode;
            else
                pred.next = newNode;
            pred = newNode;
        }

        if (succ == null) {
            last = pred;
        } else {
            pred.next = succ;
            succ.prev = pred;
        }

        size += numNew;
        modCount++;
        return true;
    }
```

#### clear()

```java
    /**
     * 从这个列表中移除所有元素。
     * 此调用返回后，列表将为空。
     */
    public void clear() {
        //清除节点间的所有链接是“不必要的”，但是：如果被丢弃的节点跨越了多个代，有助于分代垃圾回收,
        // - 即使存在可访问的迭代器，也能确保释放内存
        for (Node<E> x = first; x != null; ) {
            Node<E> next = x.next;
            x.item = null;
            x.next = null;
            x.prev = null;
            x = next;
        }
        first = last = null;
        size = 0;
        modCount++;
    }
```

#### indexOf()，lastIndexOf()

```java
    /**
     * 返回指定元素在该列表中第一次出现的索引，如果该列表不包含此元素，则返回-1。
     *
     * @param o 要搜索的元素
     * @return 在该列表中指定元素首次出现的索引，如果此列表不包含该元素，则为-1。
     */
    public int indexOf(Object o) {
        int index = 0;
        if (o == null) {
            for (Node<E> x = first; x != null; x = x.next) {
                if (x.item == null)
                    return index;
                index++;
            }
        } else {
            for (Node<E> x = first; x != null; x = x.next) {
                if (o.equals(x.item))
                    return index;
                index++;
            }
        }
        return -1;
    }

    /**
     * 返回指定元素在该列表中最后一次出现的索引，如果该列表不包含此元素，则返回-1。
     *
     * @param o 要搜索的元素
     * @return 这个列表中指定元素最后一次出现的索引，如果此列表不包含该元素，则为-1。
     */
    public int lastIndexOf(Object o) {
        int index = size;
        if (o == null) {
            for (Node<E> x = last; x != null; x = x.prev) {
                index--;
                if (x.item == null)
                    return index;
            }
        } else {
            for (Node<E> x = last; x != null; x = x.prev) {
                index--;
                if (o.equals(x.item))
                    return index;
            }
        }
        return -1;
    }
```

### HashSet&HashMap源码解析

### LinkedHashSet&Map源码解析

### TreeSet&TreeMap源码解析

## 注解

Java的注解（Annotation）是一种元数据，它允许程序员在源代码中嵌入额外的信息，这些信息可以被编译器或在运行时的Java虚拟机（JVM）使用。注解不是代码的一部分，但可以影响代码的处理方式。它们提供了将元数据与代码集成的机制，使得工具（如IDE、编译器、构建工具等）能够以声明性的方式理解代码的意图。

以下是关于Java注解的一些关键点：

1. 定义：

   - 注解以@符号开始，后面跟着注解的名称。可以使用预定义的注解（内置注解）或自定义注解。预定义注解包括@Override、@Deprecated、@ SuppressWarnings等。

2. 语法：

   - 注解可以应用于类、接口、字段、方法、构造器、参数以及局部变量上。

3. 内置注解：

   - @Override：指示方法应该覆盖超类中的方法。
   - @Deprecated：标记过时的方法或字段，编译器会发出警告。
   - SuppressWarnings：抑制特定类型的编译器警告。

4. 元注解：

   元注解（Meta-Annotations）是Java中用于注解其他注解的注解。它们提供了元数据，定义了注解的行为和用途。以下是Java中常用的元注解，以及它们的作用：

   - @Target：

     定义了注解可以应用于哪些程序元素。例如，你可以指定注解只能应用于类、方法、字段等。ElementType枚举定义了可能的目标：

     - TYPE：类、接口（包括注解类型）或枚举
     - FIELD：字段或枚举常量
     - METHOD：方法
     - PARAMETER：方法或构造器的参数
     - CONSTRUCTOR：构造器
     - LOCAL_VARIABLE：局部变量
     - ANNOTATION_TYPE：注解类型
     - PACKAGE：包
     - TYPE_PARAMETER：类型参数（Java 8及以上版本）
     - TYPE_USE：类型使用位置（如泛型参数、数组、方法返回类型等，Java 8及以上版本）

   - @Retention：

     控制注解的保留策略，决定注解在哪个阶段有效：

     - RetentionPolicy.SOURCE：注解仅存在于源代码中，编译后不保留
     - RetentionPolicy.CLASS：注解编译进字节码，但在运行时无法通过反射访问
     - RetentionPolicy.RUNTIME：注解在运行时可用，可以通过反射读取

   - @Documented：

     - 如果设置，表示该注解应该包含在生成的Javadoc中，这样用户可以通过文档了解其存在和用途。

   - @Inherited：

     - 如果设置，子类将继承父类上的该注解。但是，只有类型注解（类、接口、枚举）可以继承，成员注解（字段、方法）不会自动继承。

   - @Repeatable（Java 8及以上版本）：

     - 允许一个注解在同一位置重复出现。如果不使用此元注解，一个位置通常只能有一个特定类型的注解。使用@Repeatable，你可以定义一个容器注解来存储相同类型的一组注解。

5. 常用注解：

   - @Autowired（Spring框架）：
     - 用途：自动装配Bean，基于类型或名称注入依赖。
     - 场景：在Spring应用中，简化依赖注入，减少显式配置。
   - @Service、@Repository、@Controller（Spring框架）：
     - 用途：分别用于标记业务层、数据访问层和表现层组件。
     - 场景：Spring应用中，用于组件扫描，自动发现和配置Bean。
   - @RequestMapping、@GetMapping、@PostMapping（Spring MVC）：
     - 用途：映射HTTP请求到处理方法。
     - 场景：Web开发中，定义RESTful API的路由。
   - @Component（Spring框架）：
     - 用途：通用组件标记，用于自动检测和注册Bean。
     - 场景：Spring应用中，标记自定义组件，便于自动配置。
   - @Entity、@Table、@Column（JPA）：
     - 用途：将Java类映射到数据库表，字段映射到列。
     - 场景：ORM（对象关系映射）中，定义数据库模型。
   - @Test（JUnit）：
     - 用途：标记测试方法。
     - 场景：单元测试，标识一个方法是测试用例。

### AnnotatedElement接口

AnnotatedElement接口是Java反射API的一部分，它代表了程序中可以被注解的元素，如类、接口、方法、构造器、字段等。这个接口提供了访问附加到这些元素上的注解的方法。以下是一些关键方法：

- getAnnotation(Class<A> annotationClass)：返回指定类型的注解，如果存在的话。如果没有找到，返回null。
- isAnnotationPresent(Class<A> annotationClass)：检查是否存在指定类型的注解。
- getAnnotations()：返回所有直接附加到此元素的注解，作为一个数组。
- getDeclaredAnnotations()：返回所有直接定义在该元素上的注解，包括继承的注解。

AnnotatedElement接口是所有可以被注解的元素的父接口，如Class、Method、Constructor、Field等。例如，如果你有一个Method对象，你可以使用这个接口的方法来获取该方法上定义的注解信息。

注解本身是元数据，它们可以附加到AnnotatedElement所代表的任何编程元素上，提供有关这些元素的附加信息。这些信息可以用于编译时检查、运行时行为控制、代码生成、文档生成等多种目的。

在实际应用中，开发者通常会结合反射和注解来实现动态功能，例如在运行时检查类、方法或字段上的特定注解，然后根据这些注解的行为执行不同的逻辑。

### 自定义注解

首先，我们定义一个简单的自定义注解@MyAnnotation，它有一个字符串类型的属性value：

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyAnnotation {
    String value();
}
```

接下来，我们创建一个类AnnotatedClass，并在其中的一个方法上使用@MyAnnotation：

```java
public class AnnotatedClass {
    @MyAnnotation(value = "Hello, Annotation!")
    public void annotatedMethod() {
        // 方法逻辑
    }

    public void unannotatedMethod() {
        // 方法逻辑
    }
}
```

现在，我们可以使用反射API来检查AnnotatedClass中的annotatedMethod是否带有@MyAnnotation注解，并获取其值：

```java
public class Main {
    public static void main(String[] args) {
        try {
            Class<?> clazz = Class.forName("AnnotatedClass");
            Method annotatedMethod = clazz.getMethod("annotatedMethod");
            Method unannotatedMethod = clazz.getMethod("unannotatedMethod");

            // 检查方法是否有特定的注解
            if (annotatedMethod.isAnnotationPresent(MyAnnotation.class)) {
                MyAnnotation myAnnotation = annotatedMethod.getAnnotation(MyAnnotation.class);
                System.out.println("Annotated method: " + annotatedMethod.getName());
                System.out.println("Annotation value: " + myAnnotation.value());
            } else {
                System.out.println("Method is not annotated with @MyAnnotation.");
            }

            // 检查未被注解的方法
            if (unannotatedMethod.isAnnotationPresent(MyAnnotation.class)) {
                System.out.println("This should not happen!");
            } else {
                System.out.println("Unannotated method: " + unannotatedMethod.getName());
            }
        } catch (ClassNotFoundException | NoSuchMethodException e) {
            e.printStackTrace();
        }
    }
}
```

运行Main类，输出将是：

```java
Annotated method: annotatedMethod
Annotation value: Hello, Annotation!
Unannotated method: unannotatedMethod
```

这个例子展示了如何使用AnnotatedElement接口的isAnnotationPresent和getAnnotation方法来检查和访问自定义注解。

## 多线程与并发

### 理论基础

#### **为什么需要多线程？**

多线程的核心优势在于：

- 并行执行：充分利用多核CPU，同步执行多个任务，加速程序运行。
- 提高响应性：即使个别任务阻塞，其他任务仍可继续，确保用户界面或服务响应迅速。
- 任务分解：简化复杂任务管理，通过并发处理提升效率和可维护性。
- 并发处理：增强服务器应用的并发处理能力，有效应对高流量场景。

#### **线程不安全示例**

下面是一个简单的Java示例，展示了线程不安全的情况。在这个例子中，我们创建了一个共享的计数器类`Counter`，它有一个方法`increment`用于增加计数。然后，我们在两个线程中同时调用这个方法，期望计数器的值最终为20000，但由于缺乏同步控制，结果可能会小于20000，这是因为线程并发访问和修改共享资源时发生了竞态条件。

```java
public class Counter {
    int count = 0;

    public void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}

public class ThreadUnsafeExample {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();

        // 创建两个线程，都执行增加计数的操作
        Thread thread1 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                counter.increment();
            }
        });

        Thread thread2 = new Thread(() -> {
            for (int i = 0; i < 10000; i++) {
                counter.increment();
            }
        });

        // 启动两个线程
        thread1.start();
        thread2.start();

        // 等待两个线程都执行完毕
        thread1.join();
        thread2.join();

        // 打印最终计数器的值
        System.out.println("Final Count: " + counter.getCount());
    }
}
```

#### **并发出现问题的根源：并发三要素**

**可见性：CPU缓存引起**

可见性：当多个线程访问共享变量时，一个线程对变量的修改能够及时被其他线程看到。

示例：

```java
//线程1执行逻辑
int i = 0;
i = 10;

//线程2执行逻辑
int j = i;
```

假若执行线程1的是CPU1，执行线程2的是CPU2。线程1执行第1条指令将i初始化为0并放入主存，执行第2条指令，将i赋为10，但是这操作在CPU1的高速缓存中生效，当CPU2执行指令时，此时主存中的i值为0。

**原子性：分时复用引起**

原子性：指操作不可分割，要不全部执行成功，要不全部失败。

示例：

```java
int i = 1;

//线程1执行逻辑
i+=1;

//线程2执行逻辑
i+=1;
```

`i+=1`需要3条CPU指令：

1. 将变量 i 从内存读取到 CPU寄存器；
2. 在CPU寄存器中执行 i + 1 操作；
3. 将最后的结果i写入内存（缓存机制导致可能写入的是 CPU 缓存而不是内存）。

由于CPU分时复用（线程切换）的存在，线程1执行了第一条指令后，就切换到线程2执行，假如线程2执行了这三条指令后，再切换会线程1执行后续两条指令，将造成最后写到内存中的i值是2而不是3。

**有序性：指令重排序引起**

有序性：即程序执行的顺序按照代码的先后顺序执行。

示例：

```java
int i = 0;
boolean flag = true;
i = 1;					//语句1
flag = false;			//语句2
```

上面代码定义了一个int型变量，定义了一个boolean类型变量，然后分别对两个变量进行赋值操作。从代码顺序上看，语句1是在语句2前面的，但是JVM真正执行过程并不保证这种有序性，这里可能会发生指令重排序（Instruction Reorder）。

在执行程序时为了提高性能，编译器和处理器常常会对指令做重排序。重排序分三种类型：

- 编译器优化的重排序。编译器在不改变单线程程序语义的前提下，可以重新安排语句的执行顺序。
- 指令级并行的重排序。现代处理器采用了指令级并行技术（Instruction-Level Parallelism， ILP）来将多条指令重叠执行。如果不存在数据依赖性，处理器可以改变语句对应机器指令的执行顺序。
- 内存系统的重排序。由于处理器使用缓存和读 / 写缓冲区，这使得加载和存储操作看上去可能是在乱序执行。

从 java 源代码到最终实际执行的指令序列，会分别经历下面三种重排序：

![img](https://pdai.tech/images/jvm/java-jmm-3.png)

上述的 1 属于编译器重排序，2 和 3 属于处理器重排序。这些重排序都可能会导致多线程程序出现内存可见性问题。对于编译器，JMM 的编译器重排序规则会禁止特定类型的编译器重排序（不是所有的编译器重排序都要禁止）。对于处理器重排序，JMM 的处理器重排序规则会要求 java 编译器在生成指令序列时，插入特定类型的内存屏障（memory barriers，intel 称之为 memory fence）指令，通过内存屏障指令来禁止特定类型的处理器重排序（不是所有的处理器重排序都要禁止）。

#### JVM怎么解决并发问题

JVM通过多种机制和策略来解决并发编程中的原子性、可见性和有序性问题，确保多线程环境下的程序正确执行。以下是主要的解决手段：

**原子性问题**

1. **锁（synchronized）：**Java中的`synchronized`关键字可以用来修饰方法或代码块，确保同一时刻只有一个线程可以执行该段代码，从而保证了操作的原子性。无论是内置锁（monitor）还是显式锁（如`ReentrantLock`），都能提供互斥访问，确保并发操作的原子性。
2. **原子类（AtomicXXX）：**`java.util.concurrent.atomic`包提供了一系列原子类，如`AtomicInteger`、`AtomicBoolean`等，这些类通过CAS（Compare-and-Swap）等无锁算法实现原子操作，无需显式加锁就能保证单个变量的原子更新。

**可见性问题**

1. **volatile关键字：**使用`volatile`关键字修饰的变量，可以确保其值的修改对其他线程立即可见。volatile变量的写操作会强制将修改后的值刷新到主内存，读操作则会直接从主内存读取最新值，从而避免了缓存一致性问题。
2. **锁：**当线程释放锁时，会将工作内存的修改同步到主内存，其他线程获取锁时，会从主内存刷新最新的变量值到工作内存，这也保证了可见性。
3. **final字段规则：**对于final字段，构造函数中对其的初始化在构造函数执行完毕后对其他线程可见，即使构造函数是在不同的线程中完成的。

**有序性问题**

1. **happens-before原则：**Java内存模型（JMM）定义了一系列的happens-before规则，这些规则确定了哪些操作之间的执行顺序是不可逆的。例如，线程的start操作happens-before于该线程的每一个动作，volatile变量的写操作happens-before于后续的读操作等。这些规则确保了必要的操作顺序，避免了指令重排序带来的问题。
2. **volatile关键字：**除了保证可见性外，volatile还禁止了特定类型的发生在volatile变量读写操作之间的指令重排序，从而保证了有序性。
3. **内存屏障：**JVM在某些操作前后会自动插入内存屏障（Memory Barrier），比如在volatile读写、锁的获取与释放时，这些屏障可以阻止指令重排序，确保了操作的有序执行。

#### 线程安全保障等级

线程安全在Java中可以根据不同级别的保障强度进行分类，从强到弱依次为：

1. **不可变（Immutable）**：这是最高级别的线程安全。一旦一个对象被创建，它的状态就不能被改变。不可变对象总是线程安全的，因为它们没有可变的状态。
   - final修饰的基本数据类型
   - String类型
   - 枚举类型
   - Number 部分子类，如 Long 和 Double 等数值包装类型，BigInteger 和 BigDecimal 等大数据类型。
   - 通过Collections.unmodifiableXXX() 方法来获取一个不可变的集合。
2. **绝对线程安全（Absolutely Thread-Safe）**：在这种情况下，无论运行时环境如何，对象都能保证线程安全，调用者无需进行额外的同步措施。这意味着所有的操作都封装了必要的同步逻辑，即使在多线程并发访问下也能确保正确性。这种级别的线程安全实现成本较高，因为需要大量的同步开销。
3. **相对线程安全（Relatively Thread-Safe）**：这也是通常意义下所说的线程安全。对象本身提供了线程安全的保证，但可能需要调用者在特定情况下采取额外的同步措施来保证复合操作的原子性。例如，`Vector`和`HashTable`（在Java早期版本中）是相对线程安全的，它们的单个操作是线程安全的，但如果进行复合操作（如迭代期间的修改），则需要外部同步。
4. **线程兼容（Thread-Compat）**：这类对象不是线程安全的，但可以通过在调用端使用同步手段（如` synchronized`块）来安全地在多线程环境中使用。许多非线程安全的类，如`ArrayList`，就属于这一类。
5. **线程对立（Thread-Hostile）**：这类对象不仅不是线程安全的，而且即使在调用端进行了同步，也可能因为对象内部的设计导致无法在多线程环境中安全使用。这是一个理论上的分类，实际编程中应尽量避免设计出这样的类。

#### 线程间同步机制

线程间的同步机制是多线程编程中不可或缺的一部分，它确保了数据的一致性和操作的有序性。根据同步机制的不同特点，可以大致分为三类：互斥同步、非阻塞同步、以及无同步方案。

**互斥同步**

互斥同步是最基本的同步方式，它通过锁来实现线程间的同步，保证了同一时刻只有一个线程能访问共享资源。在Java中，最典型的互斥同步手段就是`synchronized`关键字和`ReentrantLock`等显式锁。这种同步方式简单直接，但可能会导致线程阻塞等待，从而影响系统的响应速度和吞吐量。

- **特点**：简单易用，能够确保线程安全，但可能导致线程上下文切换和阻塞开销。
- **适用场景**：适用于对实时性要求不高，且竞争不激烈的场景。

**非阻塞同步**

非阻塞同步机制通过一系列的原子操作（如CAS，即Compare and Swap）来实现线程之间的协调，避免了线程的阻塞等待。Java并发包中的`Atomic`类（如`AtomicInteger`）和一些无锁数据结构（如`ConcurrentHashMap`的部分实现）就采用了非阻塞同步策略。这种方式提高了系统的并发性能，减少了线程上下文切换的开销。

- **特点**：减少了线程阻塞，提高了系统的响应速度和吞吐量，但在高度竞争下可能会导致大量的失败重试。
- **适用场景**：适用于高并发、低延迟的场景，尤其是竞争激烈的情况下。

**无同步方案**

无同步方案是指在某些特定情况下，通过设计避免了共享数据的竞争，从而不需要额外的同步措施。比如，不可变对象（Immutable Objects）由于其状态一旦创建就不变，因此天然线程安全；局部变量（Local Variables）因每个线程有自己的栈空间，不会共享；线程本地存储（ThreadLocal）也是实现无同步的一种方式，它为每个线程提供独立的变量副本。

- **特点**：彻底避免了同步带来的开销，是最高效的线程安全方案。
- **适用场景**：适用于数据独立性高的场景，或者状态不需要跨线程共享的情况。

### 线程基础

#### 线程状态转换

![image](https://pdai.tech/images/pics/ace830df-9919-48ca-91b5-60b193f593d2.png)

线程在其生命周期中会经历以下几种状态，这些状态之间的转换描述了线程从创建到销毁的完整过程：

1. **新建（New）：**
   - 当使用`Thread`类或其子类创建了一个新的线程对象后，该线程处于新建状态。此时，它只是一个 JVM 中的对象，尚未成为一个真正的操作系统线程。
2. **就绪（Runnable）：**
   - 当调用线程对象的`start()`方法后，线程进入就绪状态。这意味着线程准备好了，等待CPU调度执行。在线程运行后，如果从等待或睡眠中返回，同样会回到就绪状态。
3. **运行（Running）：**
   - 线程调度程序从就绪状态的线程池中选择一个线程，为其分配CPU时间，使其进入运行状态，开始执行线程的`run()`方法中的代码。
4. **阻塞（Blocked）：**
   - 当线程在运行过程中请求某一锁未果（例如进入synchronized块时）、等待I/O操作完成、或者其他需要阻塞的条件出现时，线程会被动进入阻塞状态。线程在阻塞状态下不会消耗CPU时间。
5. **等待/计时等待（Waiting/Timed Waiting）：**
   - 当线程主动调用`Object.wait()`、`Thread.join()`无参方法或`LockSupport.park()`等方法时，进入等待状态；如果调用的是带有超时参数的方法（如`Thread.sleep(long millis)`、`Object.wait(long timeout)`），则进入计时等待状态。在这两种状态下，线程都需要等待特定条件满足才能继续执行。
6. **死亡（Terminated）：**
   - 线程执行完毕（包括正常结束或因异常终止），或者显式调用了`stop()`方法（已废弃，不建议使用），线程进入死亡状态，不再具有生命力，不能再次启动。

状态之间的转换关系如下：

- **新建 -> 就绪**：通过调用`start()`方法。
- **就绪 -> 运行**：线程调度程序选择就绪状态的线程执行。
- **运行 -> 就绪**：时间片用尽或更高优先级线程抢占。
- **运行 -> 阻塞/等待/计时等待**：遇到阻塞条件或调用相关方法。
- **阻塞/等待/计时等待 -> 就绪**：等待条件满足或超时到期。
- **运行 -> 死亡**：线程执行结束或因异常终止。

注意，从阻塞、等待或计时等待状态可以直接转换到运行状态，而不是必须先经过就绪状态，这取决于具体的JVM实现和操作系统调度策略。

#### 线程使用方式

在Java中，线程的使用主要有以下几种方式：

1. **继承`Thread`类**：

   - 创建一个新的类继承`Thread`类，并重写其`run()`方法，其中包含希望线程执行的任务代码。然后创建该类的实例，并调用`start()`方法来启动线程。这种方式的限制是Java不支持多重继承，如果你的类已经继承了另一个类，就不能再继承`Thread`类。

     ```java
     public class MyThread extends Thread{
         @Override
         public void run(){
             // ...
         } 
     }
     
     public class Main{
         public static void main(String[] args){
             MyThread mt = new MyThread();
             mt.start();
         }
     }
     ```

2. **实现`Runnable`接口**：

   - 创建一个类实现`Runnable`接口，并实现`run()`方法。然后将此类的实例传递给`Thread`类的构造器，创建`Thread`对象，最后调用该对象的`start()`方法来启动线程。这种方式更灵活，因为Java允许类实现多个接口。

     ```java
     public class MyRunnable implements Runnable {
         public void run() {
             // ...
         }
     }
     
     public class Main{
         public static void main(String[] args){
             MyRunnable instance = new MyRunnable();
             Thread thread = new Thread(instance);
             thread.start();
         }
     }
     ```

3. **实现`Callable`接口和使用`FutureTask`**：

   - 如果你需要线程有返回值，可以实现`Callable`接口，该接口有一个`call()`方法，它类似于`Runnable`的`run()`方法，但可以抛出检查异常并返回结果。然后将`Callable`对象封装进`FutureTask`，`FutureTask`同时实现了`Runnable`，因此可以被传给`Thread`或放入线程池执行。通过`FutureTask.get()`方法可以获取到线程执行的结果。

     ```java
     public class MyCallable implements Callable<Integer> {
         public Integer call() {
             return 123;
         }
     }
     
     public class Main{
         public static void main(String[] args) throws ExecutionException, InterruptedException {
             MyCallable mc = new MyCallable();
             FutureTask<Integer> ft = new FutureTask<>(mc);
             Thread thread = new Thread(ft);
             thread.start();
             System.out.println(ft.get());
         }
     }
     ```

4. **使用线程池（`ExecutorService`）**：

   - 通过`Executors`类提供的工厂方法创建不同类型线程池，如`newFixedThreadPool`、`newSingleThreadExecutor`、`newCachedThreadPool`等。使用线程池可以有效管理和复用线程，减少线程创建和销毁的开销，提高响应速度。提交任务到线程池使用`submit()`方法（返回`Future`用于获取结果）或`execute()`方法。

#### 基础线程机制

**Executor**

在Java中，`Executor`是一个接口，位于`java.util.concurrent`包下，它是Java并发编程框架的核心组件之一，用于解耦任务的提交与任务的执行。`Executor`框架提供了一种更加灵活和强大的方式来管理线程和任务执行，相较于传统的直接创建和管理线程的方式，它具有更高的灵活性和可维护性。

**`Executor`框架的核心概念：**

- **`Executor`接口**：这是整个框架的基础，定义了一个接收`Runnable`任务并执行的方法`void execute(Runnable command);`。这个接口非常简单，但是它的实现类和扩展接口提供了丰富的功能。
- **`ExecutorService`接口**：扩展了`Executor`，提供了更强大的功能，如提交`Callable`任务以获得`Future`结果、关闭线程池、提交批量任务等。
- **`ThreadPoolExecutor`类**：是`ExecutorService`接口的一个实现，提供了线程池的实现，允许你配置线程池的核心线程数、最大线程数、线程存活时间、任务队列等参数，是实际应用中最常用的线程池实现类。
- **`ScheduledExecutorService`接口**：进一步扩展了`ExecutorService`，添加了定时执行任务和周期性执行任务的能力。
- **`ScheduledThreadPoolExecutor`类**：实现了`ScheduledExecutorService`接口，支持定时及周期性任务的执行。

**使用`Executor`框架的好处：**

1. **资源控制**：通过线程池可以控制最大并发数，避免了创建过多线程导致的资源耗尽问题。
2. **任务管理**：可以方便地管理任务的执行，如取消任务、批量执行任务等。
3. **提高响应性**：通过复用线程，减少了线程创建和销毁的开销，提高了系统的响应速度。
4. **简化编程模型**：使用`Executor`框架，可以让开发者更多关注于任务本身而非线程管理，使得代码更加简洁清晰。
5. **灵活的线程池配置**：可以根据不同的业务场景，灵活调整线程池的参数，优化性能。

**示例代码**

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorExample {
    public static void main(String[] args) {
        // 创建一个固定大小的线程池
        ExecutorService executor = Executors.newFixedThreadPool(5);
        
        // 提交任务到线程池
        for (int i = 0; i < 10; i++) {
            Runnable worker = new WorkerThread("" + i);
            executor.execute(worker);
        }
        
        // 关闭线程池
        executor.shutdown();
        while (!executor.isTerminated()) {
        }
        System.out.println("所有任务已完成");
    }
}

class WorkerThread implements Runnable {
    private String command;

    public WorkerThread(String s) {
        this.command = s;
    }

    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " 开始. 命令 = " + command);
        processCommand();
        System.out.println(Thread.currentThread().getName() + " 结束.");
    }

    private void processCommand() {
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

上述示例展示了如何创建一个固定大小的线程池，提交任务，并在所有任务完成后关闭线程池。`Executor`框架极大地简化了并发编程的复杂度，是现代Java应用开发中处理多线程任务的标准做法。

**Daemon**

在Java中，守护线程（Daemon Thread）是一种特殊类型的线程，它的主要特点是其生命周期依赖于其他非守护线程（也称为用户线程）。当所有的非守护线程结束执行后，即使守护线程还在运行，Java虚拟机（JVM）也会自动退出，结束守护线程。守护线程通常用于执行后台支持任务，比如垃圾回收、监控内存使用情况等。

**如何设置守护线程：**

在Java中，可以通过以下方式设置一个线程为守护线程：

- 在线程创建之前，使用`Thread.setDaemon(true)`方法将其设置为守护线程。需要注意的是，这个方法必须在调用`start()`方法启动线程之前调用，否则会抛出`IllegalThreadStateException`异常。
- 另一种方式是在创建线程时，通过`Thread`构造函数的第二个参数直接指定线程是否为守护线程，但这通常不如前一种方式常见。

**示例代码：**

```java
public class DaemonThreadExample {
    public static void main(String[] args) {
        Thread daemonThread = new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    try {
                        System.out.println("守护线程正在执行...");
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }, "DaemonThread");

        // 设置为守护线程
        daemonThread.setDaemon(true);

        daemonThread.start();

        // 主线程（非守护线程）执行完毕，JVM将退出，守护线程也随之结束
        System.out.println("主线程结束");
    }
}
```

在这个例子中，`DaemonThread`是一个守护线程，它会不断打印信息并休眠1秒。当主线程结束时，尽管守护线程还在循环中，JVM也会退出，守护线程随之停止。

**注意事项：**

- 守护线程不应该用于执行任何必须完成的任务，因为它们的终止不由程序员直接控制。
- 如果在守护线程中创建新线程，默认情况下新线程是非守护线程，除非明确设置为守护线程。
- 当JVM因为只剩下守护线程而准备退出时，不会保证执行finally块或执行线程的正常中断清理工作。

守护线程在设计后台服务、资源清理、日志记录等场景中非常有用，但使用时需要谨慎，确保不影响程序的正常功能和数据完整性。

**sleep()**

Thread.sleep(millisec) 方法会休眠当前正在执行的线程，millisec 单位为毫秒。

sleep() 可能会抛出 InterruptedException，因为异常不能跨线程传播回 main() 中，因此必须在本地进行处理。线程中抛出的其它异常也同样需要在本地进行处理。

```java
public void run() {
    try {
        Thread.sleep(3000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

**yield()**

对静态方法 Thread.yield() 的调用声明了当前线程已经完成了生命周期中最重要的部分，可以切换给其它线程来执行。该方法只是对线程调度器的一个建议，而且也只是建议具有相同优先级的其它线程可以运行。

```java
public void run() {
    Thread.yield();
}
```

#### 线程中断

在Java中，线程中断是一种协作机制，用于通知线程应该停止它正在做的事情并退出执行。它并不是直接中断或停止线程的执行，而是设置一个标志位，线程需要主动检查这个标志位（中断状态）并做出响应。这一机制的设计意图是为了尊重线程的自我控制权，避免强制终止可能带来的副作用，如资源泄露或不一致的状态。

**基本概念：**

- **中断状态：**每个线程都有一个中断标志，可以通过`Thread.interrupted()`静态方法或`Thread.isInterrupted()`实例方法检查和清除。调用`Thread.interrupt()`方法会设置这个中断标志。
- **响应中断：**线程需要在适当的地方检查中断状态，比如在循环中、等待中或执行长时间任务前后。响应中断的典型做法是捕获`InterruptedException`异常或检查中断状态，并根据情况决定是否结束线程。

**中断的使用场景：**

1. **优雅地关闭线程**：当需要停止一个线程时，可以通过中断请求该线程停止执行。
2. **取消任务**：在执行长时间运算或等待IO时，可以响应中断来取消操作。
3. **超时处理**：配合等待操作（如`Thread.sleep()`, `Object.wait()`, `BlockingQueue.poll(long, TimeUnit)`)，可以实现超时后中断等待。

**示例代码：**

捕获InteruptedException

```java
public class InterruptExample extends Thread {
    @Override
    public void run() {
        try {
            while (!Thread.currentThread().isInterrupted()) {
                // 执行任务...
                Thread.sleep(1000); // 模拟耗时操作，可被中断
            }
        } catch (InterruptedException e) {
            System.out.println("线程被中断，执行清理工作...");
            Thread.currentThread().interrupt(); // 重新设置中断标志，因为catch会清除中断状态
        }
        System.out.println("线程退出");
    }

    public static void main(String[] args) throws InterruptedException {
        InterruptExample thread = new InterruptExample();
        thread.start();
        Thread.sleep(3000); // 主线程等待一段时间
        thread.interrupt(); // 请求中断线程
    }
}
```

检查中断状态

```java
public class InterruptCheckExample implements Runnable {
    @Override
    public void run() {
        while (!Thread.currentThread().isInterrupted()) {
            // 执行任务...
            // 可以定期检查中断状态，决定是否退出循环
        }
        System.out.println("线程检测到中断，准备退出...");
    }

    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(new InterruptCheckExample());
        thread.start();
        Thread.sleep(3000);
        thread.interrupt(); // 请求中断线程
    }
}
```

**注意事项：**

- 调用`Thread.interrupt()`并不意味着立即停止目标线程的执行，它只是设置中断标志。
- 需要在可能阻塞的方法调用处（如`sleep()`、`wait()`、`join()`）捕获`InterruptedException`，这些方法在接收到中断信号时会抛出此异常并清除中断状态。
- 当捕获到`InterruptedException`或检测到中断状态时，最好根据业务逻辑做出合适的响应，如清理资源、更新状态等，然后可以重新设置中断状态或直接结束线程。

#### 线程互斥同步

在Java中，线程互斥同步是一种确保多个线程在访问共享资源时，一次仅允许一个线程访问的技术，以防止数据不一致和竞态条件等问题。互斥同步主要是通过锁机制来实现，包括内置锁（也称监视器锁）和显式锁。下面详细介绍这两种同步方式：

1. **内置锁（Sychronized）**

   Java中的`synchronized`关键字提供了一种便捷的方式来实现同步，它既可以用于方法，也可以用于代码块。

   - **同步方法**：在方法声明上使用`synchronized`，这样整个方法成为同步代码块，同一时刻只允许一个线程访问该方法。

     ```java
     public synchronized void synchronizedMethod() {
         // 方法体
     }
     ```

   - **同步代码块**：更细粒度的控制，只对代码块内的内容进行同步。

     ```java
     public void method() {
         synchronized (this) { // 或者是某个对象实例
             // 需要同步的代码块
         }
     }
     ```

2. **显式锁（Lock）**

   从Java 5开始，`java.util.concurrent.locks`包提供了更灵活的锁机制，最常用的是`ReentrantLock`。与`synchronized`相比，`Lock`提供了更多的功能，如尝试获取锁、限时锁等待、公平锁等。

   - **基本使用：**

     ```java
     import java.util.concurrent.locks.Lock;
     import java.util.concurrent.locks.ReentrantLock;
     
     public class LockExample {
         private final Lock lock = new ReentrantLock();
         
         public void doSomething() {
             lock.lock(); // 获取锁
             try {
                 // 需要同步的代码块
             } finally {
                 lock.unlock(); // 释放锁，finally块确保锁总是被释放
             }
         }
     }
     ```

**特点对比：**

- **安全性**：两者都能保证线程安全，防止数据竞争。
- **灵活性**：`Lock`提供了比`synchronized`更灵活的锁操作，如尝试获取、超时获取等。
- **性能**：在大多数情况下，`Lock`和`synchronized`的性能差异不大，但在高度竞争的场景下，`Lock`可能因为其更细粒度的控制而表现更好。
- **死锁**：两者都需要注意正确使用以避免死锁，但`Lock`提供了更多的控制手段来避免死锁。

**总结：**

互斥同步是Java并发编程中的基础同步手段，通过内置锁和显式锁（如`ReentrantLock`）来实现。选择哪种同步方式应基于具体需求，如对灵活性、性能的要求以及对锁的控制精细度。正确使用互斥同步机制是保证并发程序正确性的关键。

#### 线程通信

Java线程通信是指在一个多线程程序中，不同线程之间传递信息或协调执行顺序的过程。Java提供了多种机制来实现线程间的通信，主要包括以下几种方式：

1. **共享变量**

   最直接的方式是通过共享变量来传递信息，但这种方式需要谨慎处理并发访问问题，通常需要结合同步机制（如`synchronized`、`volatile`或锁）来防止数据不一致性。

2. **wait()，notify()，notifyAll()**

   这三个方法是`Object`类的一部分，可以用来实现线程间的等待-通知模式。

   - **wait()**：使当前线程等待，并释放对象的监视器锁，直到其他线程调用该对象的`notify()`或`notifyAll()`方法。
   - **notify()**：唤醒在此对象监视器上等待的单个线程。
   - **notifyAll()**：唤醒在此对象监视器上等待的所有线程。

   这些方法必须在同步代码块或同步方法中调用，否则会抛出`IllegalMonitorStateException`异常。

   **示例代码：**

   ```java
   public class WaitNotifyExample {
       public static void main(String[] args) {
           final Object monitor = new Object();
           
           Thread producer = new Thread(() -> {
               synchronized (monitor) {
                   System.out.println("生产者准备生产...");
                   try {
                       monitor.notify(); // 唤醒消费者
                       monitor.wait(); // 生产者等待
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
                   System.out.println("生产者继续生产...");
               }
           });
   
           Thread consumer = new Thread(() -> {
               synchronized (monitor) {
                   System.out.println("消费者等待消费...");
                   try {
                       monitor.wait(); // 消费者等待
                   } catch (InterruptedException e) {
                       e.printStackTrace();
                   }
                   System.out.println("消费者开始消费...");
                   monitor.notify(); // 唤醒生产者
               }
           });
   
           consumer.start();
           producer.start();
       }
   }
   ```

3. **`java.util.concurrent`包中的高级工具**

   - **`CountDownLatch`**：允许一个或多个线程等待其他线程完成一系列操作。
   - **`CyclicBarrier`**：让一组线程等待直到到达某个屏障点，然后所有线程一起开始执行。
   - **`Semaphore`**：控制同时访问特定资源的线程数量。
   - **`Exchanger`**：两个线程之间交换对象的同步点。

   这些高级工具提供了更高级别的抽象，使得线程间的协调变得更加灵活和强大。

4. **`ConcurrentLinkedQueue`, `BlockingQueue`等并发容器**

   这些容器不仅提供了线程安全的数据存储，还内置了线程通信机制，如`BlockingQueue`的`put()`和`take()`方法会自动处理线程的等待和唤醒。

**总结**

Java线程通信机制多样，选择合适的方式取决于具体的应用场景。基础的`wait()`/`notify()`机制适合简单的同步需求，而高级并发工具则提供了更为复杂的同步控制能力，能够更高效地解决多线程间的协作问题。正确使用线程通信机制是构建高性能、高可靠的多线程应用程序的关键。

### Java并发的各种锁

Java并发编程中，为了保证线程安全，提高程序的执行效率，使用了多种锁机制来控制对共享资源的访问。

![img](https://pdai.tech/images/thread/java-lock-1.png)

#### 悲观锁 vs 乐观锁

**悲观锁**

**概念：**

悲观锁持有一种保守的态度，它假定最坏的情况会发生——即在多线程访问同一资源时，总会发生冲突。因此，在访问数据前，悲观锁会先锁定数据，阻止其他线程的访问，直到当前线程完成操作并释放锁。

**实现方式：**

在Java中，`synchronized`关键字和`ReentrantLock`（默认行为）是悲观锁的典型实现。它们都是通过独占的方式来实现同步控制，确保同一时刻只有一个线程可以执行特定的代码块或访问特定的数据。

**示例：**

```java
//synchronized关键字方式
public synchronized void synchronizedMethod() {
    // 方法体
}

//ReentrantLock方式
private final Lock lock = new ReentrantLock();
    
public void doSomething() {
    lock.lock(); // 获取锁
    try {
    	// 需要同步的代码块
    } finally {
    	lock.unlock(); // 释放锁，finally块确保锁总是被释放
	}
}
```

**乐观锁**

**概念：**

乐观锁则相对乐观，它假设读多写少的情况较为常见，并且在大部分情况下，多个线程对数据的访问不会发生冲突。因此，乐观锁并不会在访问数据前直接加锁，而是在更新数据时检查在此期间数据是否被其他线程修改过，如果数据未被修改，则更新成功；如果已被修改，则通常会重试整个操作。

**实现方式：**

乐观锁在Java中的常见实现方式不依赖于语言层面的锁机制，更多地是一种逻辑上的实现。一种常见的实现是使用版本号（Version）或时间戳（Timestamp）。例如，在数据库中，可以在表中添加一个version字段，每次更新时，同时更新version值。在更新数据前，先读取version，更新时将version作为条件一起更新，如果version值与之前读取的不一致，则说明数据已被其他线程修改，更新失败。

**示例：**

乐观锁的一个典型应用是使用 `java.util.concurrent.atomic` 包下的原子类，比如 `AtomicInteger`，它利用CAS（Compare and Swap）操作实现无锁的线程安全更新：

```java
import java.util.concurrent.atomic.AtomicInteger;

public class Counter {
    private AtomicInteger count = new AtomicInteger(0);

    public void increment() {
        count.incrementAndGet();
    }

    public int getCount() {
        return count.get();
    }
}
```

在这个例子中，`incrementAndGet` 方法使用了CAS操作来原子性地增加 `count` 的值并获取最新的值。如果有其他线程在此期间修改了 `count`，那么CAS操作会失败并重新尝试，直到成功。这种方式避免了传统的加锁开销，更加高效，体现了乐观锁的“先操作后验证”的理念。

<img src="https://pdai.tech/images/thread/java-lock-2.png" alt="img" style="zoom: 67%;" />

**对比**

- **性能：** 在并发量不大、写操作较少的场景下，乐观锁因为减少了锁的开销，通常能提供更好的性能。而在高并发写操作频繁的场景下，乐观锁因频繁的重试可能会导致性能下降。
- **安全性：** 悲观锁通过阻塞其他线程来保证数据的一致性，因此在并发控制上更为严格和安全。而乐观锁依赖于数据版本的比较，如果控制不当，可能在并发冲突检测和解决上存在风险。
- **适用场景：** 乐观锁更适合于读多写少、并发冲突概率较低的场景，如多用户编辑但最终提交较少的情况。悲观锁则适用于写操作频繁、并发冲突较高的场景，如银行账户转账等对数据一致性要求极高的操作。

#### 自旋锁 vs 适应性自旋锁

**自旋锁**

自旋锁是一种简单的同步机制，当一个线程尝试获取一个已被其他线程持有的锁时，不是立即放弃CPU控制权并进入阻塞状态，而是通过循环（自旋）不断地检查锁的状态。如果持有锁的线程能在很短时间内释放锁，那么自旋的线程就可以避免阻塞和上下文切换的开销，迅速获得锁并继续执行。然而，如果锁被持有时间较长，自旋锁就会浪费CPU资源，因为自旋线程一直在执行无用的循环操作。

**适应性自旋锁**

适应性自旋锁是对自旋锁的一种优化，它加入了动态调整自旋时间的机制。在Java中，从JDK 1.6开始，HotSpot虚拟机引入了适应性自旋锁。适应性自旋锁会根据上一次自旋是否成功、以及当前CPU的负载情况动态调整自旋的次数。如果在过去的执行过程中，自旋很少成功获得锁（意味着锁被持有的时间往往较长），那么后续的自旋次数将会减少甚至直接省略自旋进入阻塞状态。反之，如果自旋经常能够快速获得锁，那么将会延长自旋时间。

适应性自旋锁还会参考操作系统的当前线程状态，以及CPU核心的数量来决定是否需要进行自旋以及自旋的持续时间。这样，自旋锁在不同的运行环境和工作负载下能有更优的性能表现，减少无谓的CPU消耗。

<img src="https://pdai.tech/images/thread/java-lock-4.png" alt="img" style="zoom: 50%;" />

**总结**

- **自旋锁**简单直接，但在锁持有时间不确定或较长时可能导致CPU浪费。
- **适应性自旋锁**通过动态调整自旋策略，能够在多种场景下达到更好的性能平衡，尤其在轻量级锁竞争和多核CPU环境下更为有效，因为它能够减少不必要的自旋，避免无谓的CPU占用，同时保持了对于短时锁争用的良好响应速度。

#### 无锁 vs 偏向锁 vs 轻量级锁 vs 重量级锁

<img src="https://pdai.tech/images/thread/java-lock-6.png" alt="img" style="zoom: 50%;" />

这四种锁机制（无锁、偏向锁、轻量级锁、重量级锁）主要是针对Java中`synchronized`关键字的实现进行的优化策略。在Java SE 6及以后的版本中，为了提高`synchronized`的性能，JVM引入了这些不同的锁状态，以便在不同场景下自动选择最合适的同步策略。

- **无锁**虽然不是一个特定的`synchronized`状态，但它代表了一种不需要显式锁的并发控制方式，如使用`Atomic`类进行线程安全操作，与`synchronized`机制并行存在，提供了一种高性能的并发处理方式。
- **偏向锁**、**轻量级锁**、**重量级锁**则是`synchronized`内部实现的锁升级路径，用于处理不同程度的线程竞争：
  - **偏向锁**期望锁大多数时间被单一线程拥有，减少无竞争情况下的同步开销。
  - **轻量级锁**通过自旋尝试获取锁，适用于线程竞争不激烈的场景。
  - **重量级锁**是当锁竞争变得激烈，轻量级锁自旋无效时的最后手段，会涉及操作系统层面的线程阻塞与唤醒，开销较大。

JVM会根据锁的竞争情况动态地在这几种状态间转换，以实现最佳的性能平衡。这些优化策略是`synchronized`机制的一部分，旨在提升并发程序的执行效率。

#### 公平锁 vs 非公平锁

在Java并发编程中，公平锁（Fair Lock）和非公平锁（Nonfair Lock）是两种不同属性的锁，主要区别在于它们对待线程获取锁的顺序处理方式上。

**公平锁（Fair Lock）**

**定义：**公平锁严格按照线程请求锁的顺序来分配锁，即先来先得。它维护了一个先进先出（FIFO）的等待队列，确保线程按照其请求锁的顺序依次获得锁。这保证了每个线程都能得到公平对待，减少了“饥饿”现象，即线程不会无限期地等待。

**特点：**

- 提供了可预测的锁获取顺序。
- 相对非公平锁，吞吐量可能较低，因为需要维护线程队列，增加了上下文切换的成本。
- 实现较为复杂，需要维护线程队列。

<img src="https://pdai.tech/images/thread/java-lock-7.png" alt="img" style="zoom:50%;" />

**非公平锁（Nonfair Lock）**

**定义：** 非公平锁在尝试获取锁时，允许插队行为。即使有线程已经在等待队列中，新来的线程仍有机会直接获取锁（通常是通过CAS操作尝试立即获取）。这意味着即使是后来的线程，只要有机会（比如锁刚被释放），也可能比队列中等待的线程更快获得锁。

**特点：**

- 相对于公平锁，通常有更好的吞吐量，因为减少了线程的阻塞等待时间。
- 可能会导致某些线程“饥饿”，长时间等待的线程可能一直得不到锁。
- 实现相对简单，因为不必严格维护线程的等待顺序。

<img src="https://pdai.tech/images/thread/java-lock-8.png" alt="img" style="zoom:50%;" />

**示例**

在Java中，`ReentrantLock`类可以通过构造函数参数来指定是否为公平锁，默认是非公平锁。

```java
// 创建公平锁
ReentrantLock fairLock = new ReentrantLock(true);

// 创建非公平锁（默认）
ReentrantLock nonFairLock = new ReentrantLock();
```

**应用场景**

- **公平锁**适用于那些对锁获取顺序有严格要求，且能容忍较低吞吐量的场景。
- **非公平锁**适合于那些重视吞吐量，对锁获取顺序不敏感，或者线程等待时间差异不大的场景。

#### 可重入锁 vs 非可重入锁

在Java并发编程中，可重入锁（Reentrant Lock）和非可重入锁（Non-reentrant Lock）是两种不同特性的锁机制，它们在多线程环境中的表现和用途有所区别。

**可重入锁（Reentrant Lock）**

**定义：**可重入锁允许同一个线程多次获取同一把锁而不被阻塞。也就是说，如果一个线程已经持有了某个对象的锁，那么在该锁的保护区内再次请求这个锁时，该线程可以顺利获得锁。Java中的`synchronized`关键字和`ReentrantLock`类都是可重入锁的典型实现。

**特点：**

- 避免了死锁：可重入特性避免了线程因自己持有锁而无法再次进入的情况，从而降低了死锁的风险。
- 递归调用友好：支持线程在持有锁的情况下调用自身或同类中其他同步方法。
- 状态计数：可重入锁内部通常会维护一个计数器来记录锁被重入的次数，每次成功获取锁计数器加1，每次释放锁计数器减1，直到计数器为0时锁才真正释放。

<img src="https://pdai.tech/images/thread/java-lock-12.png" alt="img" style="zoom:50%;" />

**非可重入锁（Non-reentrant Lock）**

**定义：**非可重入锁则不允许同一个线程重复获取同一把锁。如果一个线程已经持有一个非可重入锁，再次尝试获取该锁时，即使是同一个线程也会被阻塞，直到前一次获取的锁被释放。

**特点：**

- 死锁风险：在实际应用中较少直接使用非可重入锁，因为它可能导致线程因自己持有锁而死锁。
- 设计复杂度：非可重入锁在设计时需要特别小心，以避免死锁和递归调用的问题。
- 使用场景有限：通常用于一些特定场景，如某些嵌入式系统或特定的同步逻辑中，且确保不会有自我递归调用或明确知道不会出现锁重入的情况。

<img src="https://pdai.tech/images/thread/java-lock-13.png" alt="img" style="zoom:50%;" />

**应用**

Java标准库中广泛采用可重入锁，因为它更加安全且易于使用。`synchronized`块和方法默认就是可重入的，而`java.util.concurrent.locks.ReentrantLock`类也是作为可重入锁提供给开发者使用的，尽管它允许通过构造函数选择是否为公平锁，但其可重入特性始终存在。

非可重入锁在Java标准库中并不常见，因为它的使用复杂且容易引发问题，但在某些特定的第三方库或特殊场景下可能会有应用。一般情况下，开发者更多地考虑如何合理使用可重入锁来设计并发程序。

#### 共享锁 vs 排他锁

在Java并发编程中，共享锁（Shared Lock）和排他锁（Exclusive Lock）是两种基本的锁类型，它们在并发控制中扮演着核心角色，用于管理对共享资源的访问权限，确保数据的一致性和线程安全。

**排他锁（Exclusive Lock）**

- **定义**：排他锁，也称为互斥锁，只允许一个线程持有。一旦某个线程获得了排他锁，其他所有请求该锁的线程都必须等待，直到锁被释放。`synchronized`关键字和`ReentrantLock`在默认情况下提供的就是排他锁。
- **特点**：确保了同一时刻只有一个线程可以执行受保护的代码段，适用于写操作，或任何需要独占资源的场景。
- **用途**：适用于需要修改共享资源的场景，如增加、删除或修改数据，以防止并发修改导致的数据不一致性。

**共享锁（Shared Lock）**

- **定义**：共享锁允许多个线程同时持有，只要这些线程都是进行读操作。这意味着多个读线程可以同时访问被共享锁保护的资源，但一旦有线程需要写入，所有读线程和其他写线程都必须等待。
- **特点**：提高了并发读的性能，因为多个读操作可以并发执行，但写操作仍然是独占的。
- **用途**：适用于读多写少的场景，可以提高系统的并发读取能力，如在数据库中，多个查询可以同时读取同一份数据，但任何写入请求必须等待所有读取完成。

**实现与应用**

在Java中，`java.util.concurrent.locks.ReentrantReadWriteLock`是一个典型的实现，它提供了分离的读锁（共享锁）和写锁（排他锁）。读锁可以在多个线程间共享，而写锁则是独占的。这种设计允许并发读取，但写入时会排斥所有的读取和写入操作，确保了数据的完整性和一致性。

**总结**

选择共享锁还是排他锁取决于具体的应用场景和对数据一致性的要求。在读操作远多于写操作的场景下，共享锁可以显著提高系统的并发性能。而对于需要更新数据的操作，则应使用排他锁来确保数据的独占访问，防止并发写冲突。正确地运用这两种锁机制，是实现高性能并发系统的关键。

### 关键字：sychronized详解

### 关键字：volatile详解

### 关键字：final详解

### JUC概述

### JUC原子类

### JUC锁

### JUC集合

### JUC线程池

### JUC工具类

## IO/NIO/AIO

### 输入/输出概念

在计算机科学和编程领域，"输入"和"输出"的概念是从程序或系统的角度来看的。

- **输入（Input）**：指的是程序或系统从外部接收数据或指令的过程。这些数据或指令可以来源于用户通过键盘输入、文件读取、网络数据包接收、传感器读数等多种途径。简而言之，输入是程序为了执行任务而从外界获取信息的行为。
- **输出（Output）**：则是指程序或系统向外发送数据或结果的过程。输出可以是显示在屏幕上的信息、写入文件的内容、通过网络发送的数据包或是控制硬件的动作等。它是程序执行结果的表现形式，或者是与外部环境交流的方式。

所以，当我们谈论程序的输入和输出时，我们是指数据流动的方向相对于程序本身而言。"输入"是向程序内部流入的信息，而"输出"是从程序内部流出到外部世界的信息或效应。这个概念适用于各种软件和硬件系统，是计算机处理信息的基本模式之一。

### Java IO - 分类

Java I/O（Input/Output，输入/输出）是Java语言中处理数据输入和输出的一个重要部分，它使得程序能够与外部设备（如文件、网络、控制台等）进行交互。

#### 数据操作

从数据操作的角度分类，Java I/O可以分为以下几个主要类别：

1. **字节流（Byte Streams）：**
   - **输入流**：如 `InputStream` 及其子类（如 `FileInputStream`, `ByteArrayInputStream`），用于从源读取字节数据。
   - **输出流**：如 `OutputStream` 及其子类（如 `FileOutputStream`, `ByteArrayOutputStream`），用于向目标写入字节数据。
2. **字符流（Character Streams）：**
   - **输入流**：如 `Reader` 及其子类（如 `FileReader`, `CharArrayReader`），用于读取字符数据，支持字符编码转换。
   - **输出流**：如 `Writer` 及其子类（如 `FileWriter`, `CharArrayWriter`），用于写入字符数据，同样支持字符编码。
3. **对象序列化与反序列化：**
   - 使用 `ObjectOutputStream` 进行对象的序列化操作，将对象转换为字节流以便存储或传输。
   - 使用 `ObjectInputStream` 进行对象的反序列化操作，将字节流还原为对象。
4. **缓冲流（Buffered Streams）：**
   - 在原始字节流或字符流基础上添加缓冲区，如 `BufferedInputStream`, `BufferedOutputStream`, `BufferedReader`, `BufferedWriter`，以提高读写效率。
5. **转换流（Conversion Streams）：**
   - 如 `InputStreamReader` 和 `OutputStreamWriter`，用于在字节流与字符流之间进行转换，支持字符编码的处理。
6. **打印流（Print Streams）：**
   - 如 `PrintStream` 和 `PrintWriter`，提供格式化的输出功能，常用于输出到控制台或其他输出流。
7. **数据流（Data Streams）：**
   - 如 `DataInputStream` 和 `DataOutputStream`，用于读写基本数据类型，如整数、浮点数等。
8. **随机访问文件（Random Access Files）：**
   - 通过 `RandomAccessFile` 类，允许随机读写文件中的数据，支持文件的读、写或读写模式。
9. **NIO（New Input/Output）：**
   - 引入了通道（Channels）、缓冲区（Buffers）和选择器（Selectors），支持非阻塞I/O，提供更高的数据传输效率，特别是在处理大量并发连接时。

这些分类帮助开发者根据数据的类型（字节或字符）、操作需求（如是否需要缓冲、是否涉及对象序列化等）以及性能要求来选择合适的I/O类进行数据操作。

#### 传输方式

从传输方式的角度分类，Java I/O可以分为以下几种类型：

1. **阻塞I/O（Blocking I/O）**：
   - 在这种模式下，当一个线程发起I/O操作（如读或写），如果数据没有立即准备好，该线程会被挂起，直到操作完成。Java传统的I/O操作，如使用`InputStream`、`OutputStream`、`Reader`、`Writer`等进行文件或网络操作时，默认采用阻塞模式。这种方式简单易用，但在高并发场景下可能会导致线程资源浪费。
2. **非阻塞I/O（Non-blocking I/O, NIO）**：
   - 引入于Java 1.4的NIO，允许线程在数据未准备好时不必等待，可以继续执行其他任务。NIO使用通道（Channels）和缓冲区（Buffers）进行数据传输，配合选择器（Selectors）可以监控多个通道的事件（如读就绪、写就绪），实现单线程管理多个连接，提高了系统吞吐量和处理能力。
3. **多路复用I/O（Multiplexed I/O）**：
   - 这是NIO非阻塞模型的一个关键特性，特别是通过选择器（Selectors）实现。一个选择器可以同时监控多个通道的I/O事件，当任何一个通道准备好进行读或写操作时，选择器就会通知应用程序，从而一个单独的线程可以管理多个连接，减少了线程上下文切换的开销。
4. **异步I/O（Asynchronous I/O, AIO）**：
   - 引入于Java 7，AIO提供了更高级别的非阻塞操作。在AIO模型中，当发起一个I/O操作后，线程不需要等待操作完成，而是注册一个回调函数（CompletionHandler），当I/O操作完成后，操作系统会直接调用回调函数通知应用程序，实现了真正的异步处理，进一步提高了应用的响应速度和吞吐量。
5. **内存映射文件I/O**：
   - 虽不是严格意义上的传输方式分类，但也是NIO中一个重要的特性。通过`FileChannel`的`map()`方法，可以将文件的一部分或全部映射到内存中，使文件操作如同直接访问内存一样快速，适用于大文件的高效读写。

每种传输方式都有其适用场景，开发者可以根据应用的具体需求选择最合适的I/O模型，以达到最佳的性能和资源利用率。

### Java IO - InputStream源码

`InputStream` 是Java I/O（输入/输出）库中的一个核心抽象类，位于`java.io`包中。它是所有字节输入流的超类，定义了一套标准的方法来读取字节数据，但并不直接提供实现。由于是抽象类，你不能直接实例化`InputStream`对象，而需要使用它的子类，如`FileInputStream`、`ByteArrayInputStream`、`BufferedInputStream`等，这些子类针对不同的数据来源提供了具体的实现。

#### InputStream源码

```java
/**
 * 这个抽象类是所有表示字节输入流的类的超类
 * 需要定义`InputStream`子类的应用程序必须始终提供一个方法来返回输入的下一个字节
 *
 * @author  Arthur van Hoff
 * @see     java.io.BufferedInputStream
 * @see     java.io.ByteArrayInputStream
 * @see     java.io.DataInputStream
 * @see     java.io.FilterInputStream
 * @see     java.io.InputStream#read()
 * @see     java.io.OutputStream
 * @see     java.io.PushbackInputStream
 * @since   JDK1.0
 */
public abstract class InputStream implements Closeable {
    
	//MAX_SKIP_BUFFER_SIZE 用于确定跳过时使用的最大缓冲区大小
    private static final int MAX_SKIP_BUFFER_SIZE = 2048;
    
     /**
     * 从输入流中读取下一个字节的数据
     * 该字节的值以int的形式返回，范围为0~255
     * 如果由于到达流的末尾而没有可用的字节，则返回值为-1 
     * 这个方法会阻塞直到输入数据可用、检测到流的结束或抛出异常
     *
     * 子类必须提供此方法的实现
     *
     * @return     下一个字节的数据，如果到达流的末尾则为-1
     * @exception  IOException  如果发生I/O错误
     */
    public abstract int read() throws IOException;
    
    /**
    * 从输入流中读取一定数量的字节，并将它们存储到缓冲区数组b中
    * 实际读取的字节数作为整数返回
    * 此方法会一直阻塞，直到有输入数据可用、检测到文件结束或抛出异常
    * 
    * 如果 b 的长度为零，则不读取任何字节并返回 0；否则，尝试至少读取一个字节。
    * 如果因为流已到达文件末尾而没有可用的字节，则返回值 -1；否则，至少读取一个字节并存储到 b 中。
    *
    * 读取的第一个字节存储在元素b[0]中，下一个字节存储在b[1]中，以此类推。
    * 读取的字节数最多等于b的长度。
    * 设k为实际读取的字节数；这些字节将被存储在元素b[0]到b[k-1]中，使得元素b[k]到b[b.length-1]不受影响。
    *
    * InputStream类中的read(b)方法与以下操作效果相同：
    * `read(b,0,b.length)`
    *
    * Params：b – 用于读取数据的缓冲区
    * Returns：缓冲区中读取的总字节数，如果已达到流的末尾而没有更多数据，则为-1
    * Throws：IOExceptin - 如果第一个字节无法被读取，且原因不包括文件结束、输入流已被关闭，或发生了其他I/O错误
    * 		  NullPointerException - 如果b为null
    * See Also：read(byte[],int,int)
    */
    public int read(byte b[]) throws IOException {
        return read(b, 0, b.length);
    }
}

```

