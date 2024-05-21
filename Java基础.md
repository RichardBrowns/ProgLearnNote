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

的继承（Inheritance）是一种创建新类的方式，新类（子类、派生类）继承了现有类（父类、基类）的特性和行为，并可以在此基础上添加新的特性或重写父类的方法。继承是面向对象的四大基本原则（封装、继承、多态、抽象）之一，其核心目的和特点包括：

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

   - 枚举类型默认就是final的，不能被继承，且枚举常量的值也是不可变的。、

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
   - 验证：确保类文件的正确性，没有违反JVM规范
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
   - public static final Class<?>[] EMPTY_CLASS_ARRAY = {};：一个空的 Class 数组，常用于避免创建空数组的开销
   - private static final int SYNTHETIC = 0x00001000;：标志位，表示类或成员是编译器生成的（比如桥接方法）
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

**核心接口继承关系**

1. Collection -- 所有集合的根接口
   - List -- 有序、可重复
     - ArrayList，LinkedList
   - Set -- 无序、不可重复
     - HashSet，LinkedHashSet，TreeSet
2. Map -- 键值对（key，value）集合
   - HashMap，LinkedHashMap，TreeMap...

**特殊接口**

- Iterable -- 此接口被Collection继承，为不同的集合提供统一的遍历方式
- Cloneable和Serializable -- 许多集合类实现了这些接口以支持克隆和序列化

**不同集合的特点**

- ArrayList -- 动态数组实现，随机访问快，插入和删除慢
- LinkedList -- 双向链表，插入和删除块，随机访问慢
- HashSet -- 基于哈希表实现，无序，不允许重复
- TreeSet -- 基于红黑树实现，有序，不允许重复
- HashMap -- 哈希表实现，键值对，无序
- TreeMap -- 红黑树实现，键值对，有序

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

## IO/NIO/AIO