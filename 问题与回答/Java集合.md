# 概述

## 有哪些常见的集合框架？

在Java中，集合框架是一个用于存储和操作数据组的架构。它提供了一组接口和类来支持操作不同类型的集合。以下是一些常见的集合框架及其简要介绍：

1. **List接口**

   `List`接口表示一个有序的集合（也称为序列），其中的元素可以重复。常见的实现类包括：

   - **ArrayList**：基于数组的实现，支持快速随机访问和遍历操作，但在插入和删除操作时性能较差（特别是在中间位置插入或删除元素时）。
   - **LinkedList**：基于双向链表的实现，支持快速的插入和删除操作，但随机访问性能较差。
   - **Vector**：类似于`ArrayList`，但它是同步的（线程安全），因此在多线程环境中使用时性能较差。

2. **Set接口**

   `Set`接口表示一个不包含重复元素的集合。常见的实现类包括：

   - **HashSet**：基于哈希表的实现，不保证集合的迭代顺序。
   - **LinkedHashSet**：继承自`HashSet`，但它维护了一个双向链表来记录元素的插入顺序，因此迭代顺序与插入顺序一致。
   - **TreeSet**：基于红黑树的实现，保证集合元素的自然顺序或根据提供的比较器进行排序。

3. **Queue接口**

   `Queue`接口表示一个先进先出的集合。常见的实现类包括：

   - **LinkedList**：可以作为队列使用，支持双向队列操作。
   - **PriorityQueue**：基于优先级堆的实现，元素按优先级顺序排列。

4. **Deque接口**

   `Deque`接口表示一个双端队列，可以从两端插入和删除元素。常见的实现类包括：

   - **ArrayDeque**：基于数组的双端队列实现，支持快速的插入和删除操作。
   - **LinkedList**：也可以作为双端队列使用。

5. **Map接口**

   `Map`接口表示一个键值对的集合，其中每个键最多映射到一个值。常见的实现类包括：

   - **HashMap**：基于哈希表的实现，不保证映射的顺序。
   - **LinkedHashMap**：继承自`HashMap`，但它维护了一个双向链表来记录键值对的插入顺序，因此迭代顺序与插入顺序一致。
   - **TreeMap**：基于红黑树的实现，保证键的自然顺序或根据提供的比较器进行排序。
   - **Hashtable**：类似于`HashMap`，但它是同步的（线程安全），因此在多线程环境中使用时性能较差。

6. **Concurrent Collections**

   Java还提供了一些线程安全的集合类，用于并发编程环境：

   - **ConcurrentHashMap**：线程安全的哈希表实现，支持高效的并发访问。
   - **CopyOnWriteArrayList**：线程安全的`ArrayList`实现，适合读操作远多于写操作的场景。
   - **CopyOnWriteArraySet**：线程安全的`Set`实现，适合读操作远多于写操作的场景。

7. **其他集合类**

   - **Stack**：继承自`Vector`，表示一个后进先出的栈。
   - **EnumSet**：专门为枚举类型设计的高效集合实现。

# List

## ArrayList和LinkedList的区别？

二者的区别如下：

1. **数据结构**
   - **ArrayList：**基于动态数组实现。如果数组满了，就会创建一个更大的数组并把旧数据复制过去。
   - **LinkedList：**基于双向链表实现。每一个节点都包含对前一个和后一个节点的引用。
2. **存取元素的时间复杂度**
   - **ArrayList：**由于数组支持随机访问，`get`和`set`操作的时间复杂度是`O(1)`。
   - **LinkedList：**由于需要从头或尾部遍历链表找到指定位置，`get`和`set`操作的时间复杂度是`O(n)`。
3. **插入和删除元素的时间复杂度**
   - **ArrayList**: 在数组的末尾添加元素（`add`操作）平均时间复杂度是`O(1)`，但在中间插入或删除元素时，需要移动后续元素，时间复杂度是`O(n)`。
   - **LinkedList**: 在链表的头部或尾部添加或删除元素（`add`和`remove`操作）时间复杂度是`O(1)`，但在中间插入或删除元素时，需要遍历链表找到指定位置，时间复杂度是`O(n)`。
4. **内存使用**
   - **ArrayList**: 由于数组需要分配连续的内存空间，可能会导致内存碎片问题。当数组需要扩容时，会创建一个新的更大的数组并复制旧数组的数据。
   - **LinkedList**: 每个节点需要额外的内存来存储对前一个和后一个节点的引用，因此内存开销比`ArrayList`大。
5. **迭代器性能**
   - **ArrayList**: 由于底层是数组，迭代器的性能较好，尤其是随机访问。
   - **LinkedList**: 由于底层是链表，迭代器的性能较差，尤其是随机访问。
6. **特殊操作**
   - **ArrayList**: 支持高效的随机访问和批量操作（如`subList`）。
   - **LinkedList**: 提供了一些特有的方法，如`addFirst`、`addLast`、`removeFirst`和`removeLast`，适合用作队列和双端队列（Deque）。
7. **线程安全**
   - **ArrayList**: 不是线程安全的。如果需要线程安全的`ArrayList`，可以使用`Collections.synchronizedList(new ArrayList<>())`。
   - **LinkedList**: 也不是线程安全的。如果需要线程安全的`LinkedList`，可以使用`Collections.synchronizedList(new LinkedList<>())`。

**适用场景**

- **ArrayList**: 适用于需要频繁随机访问元素的场景，或者主要进行尾部添加和删除操作的场景。
- **LinkedList**: 适用于需要频繁在列表中间进行插入和删除操作的场景，或者需要使用队列或双端队列功能的场景。

## ArrayList的扩容机制？

ArrayList的底层是一个数组，当元素超过当前数组的容量时，`ArrayList`会自动扩容以容纳更多的元素。

过程如下：

1. **创建一个新的更大的数组**：新的数组的容量通常是旧数组容量的 1.5 倍（具体倍数可能因实现版本不同而略有不同）。
2. **将旧数组的元素复制到新数组中**：使用 `System.arraycopy` 方法将旧数组中的元素复制到新数组中。
3. **将新数组的引用赋值给 `ArrayList` 的底层数组**：旧数组将被垃圾回收机制回收。

具体如下：

1. **默认初始容量**：`ArrayList` 的默认初始容量为 10。
2. **扩容倍数**：当需要扩容时，新容量为旧容量的 1.5 倍（即 `newCapacity = oldCapacity + (oldCapacity >> 1)`）。
3. **最大容量**：如果新容量超过了 `Integer.MAX_VALUE - 8`，则新容量将被设置为 `Integer.MAX_VALUE`。

## ArrayList怎么序列化？

在 Java 中，`ArrayList` 类已经实现了 `java.io.Serializable` 接口，这意味着它可以直接被序列化和反序列化。序列化是指将对象的状态转换为字节流的过程，从而可以将对象存储到文件中或通过网络传输。反序列化是指将字节流转换回对象的过程。

**ArrayList的序列化**

`ArrayList` 的序列化过程是通过 `writeObject` 和 `readObject` 方法来实现的。这些方法会处理 `ArrayList` 的内部数据结构，将其转换为字节流或从字节流中恢复。

**transient修饰符**

`transient` 关键字用于声明不希望被序列化的字段。在 `ArrayList` 中，有些内部字段是用 `transient` 修饰的，例如 `elementData` 数组。`elementData` 是存储 `ArrayList` 元素的实际数组。

**为什么用 `transient` 修饰 `elementData` 数组？**

1. **避免冗余数据**：`elementData` 数组的容量通常大于 `ArrayList` 实际存储的元素数量。序列化整个数组会导致不必要的空间浪费。通过使用 `transient` 关键字，`elementData` 数组不会被直接序列化，只有实际存储的元素会被序列化。
2. **自定义序列化逻辑**：`ArrayList` 通过实现 `writeObject` 和 `readObject` 方法，提供了自定义的序列化和反序列化逻辑。在这些方法中，`ArrayList` 只序列化实际存储的元素，而不是整个 `elementData` 数组。

**实现ArrayList的序列化和反序列化**

```java
// 序列化
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;
import java.util.ArrayList;

public class SerializeArrayList {
    public static void main(String[] args) {
        // 创建一个 ArrayList 并添加一些元素
        ArrayList<String> list = new ArrayList<>();
        list.add("Apple");
        list.add("Banana");
        list.add("Cherry");

        try {
            // 创建文件输出流
            FileOutputStream fileOut = new FileOutputStream("arraylist.ser");
            // 创建对象输出流
            ObjectOutputStream out = new ObjectOutputStream(fileOut);
            // 序列化 ArrayList
            out.writeObject(list);
            // 关闭流
            out.close();
            fileOut.close();
            System.out.println("Serialized data is saved in arraylist.ser");
        } catch (IOException i) {
            i.printStackTrace();
        }
    }
}

// 反序列化
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;
import java.util.ArrayList;

public class DeserializeArrayList {
    public static void main(String[] args) {
        ArrayList<String> list = null;

        try {
            // 创建文件输入流
            FileInputStream fileIn = new FileInputStream("arraylist.ser");
            // 创建对象输入流
            ObjectInputStream in = new ObjectInputStream(fileIn);
            // 反序列化 ArrayList
            list = (ArrayList<String>) in.readObject();
            // 关闭流
            in.close();
            fileIn.close();
        } catch (IOException i) {
            i.printStackTrace();
            return;
        } catch (ClassNotFoundException c) {
            System.out.println("ArrayList class not found");
            c.printStackTrace();
            return;
        }

        // 打印反序列化后的 ArrayList
        for (String item : list) {
            System.out.println(item);
        }
    }
}
```

**总结**

- `ArrayList` 实现了 `Serializable` 接口，因此可以直接进行序列化和反序列化。
- `transient` 关键字用于声明不希望被序列化的字段。在 `ArrayList` 中，`elementData` 数组被 `transient` 修饰，以避免序列化冗余数据并实现自定义序列化逻辑。

## 快速失败（Fail-Fast）和安全失败（Fail-Safe）？

快速失败（fail-fast）和安全失败（fail-safe）是两种不同的错误处理策略。

**快速失败（Fail-Fast）**

快速失败策略的核心思想是，当系统检测到潜在错误或不一致状态时，立即报告错误并停止运行。这种策略有助于在问题发生时迅速发现和修复。

**特点**

1. **即时报告**：一旦检测到问题，系统会立即抛出异常或错误。
2. **简化调试**：由于错误被迅速发现和报告，调试和修复问题更加容易。
3. **严格一致性**：快速失败策略通常用于需要严格一致性的场景中。

**示例**

在 Java 中，集合框架中的迭代器通常是快速失败的。例如，如果在迭代过程中对集合进行结构修改（如添加或删除元素），迭代器会抛出 `ConcurrentModificationException`。

```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class FailFastExample {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("A");
        list.add("B");
        list.add("C");

        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
            // 修改集合
            list.add("D");
        }
    }
}
```

运行上述代码会抛出 `ConcurrentModificationException`，因为在迭代过程中修改了集合。

**安全失败（Fail-Safe）**

安全失败策略的核心思想是，即使系统检测到潜在错误或不一致状态，也会尽量继续运行，并在后台记录或处理错误。这种策略有助于提高系统的健壮性和容错能力。

**特点**

1. **容错能力强**：即使出现错误，系统也会尽量继续运行。
2. **延迟报告**：错误可能不会立即抛出，而是记录在日志中或通过其他方式报告。
3. **适用于松散一致性**：安全失败策略通常用于不需要严格一致性的场景中。

**示例**

在 Java 中，`CopyOnWriteArrayList` 是一个安全失败的集合类。它的迭代器不会抛出 `ConcurrentModificationException`，因为它在迭代过程中使用了一个快照（snapshot）。

```java
import java.util.Iterator;
import java.util.concurrent.CopyOnWriteArrayList;

public class FailSafeExample {
    public static void main(String[] args) {
        CopyOnWriteArrayList<String> list = new CopyOnWriteArrayList<>();
        list.add("A");
        list.add("B");
        list.add("C");

        Iterator<String> iterator = list.iterator();
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
            // 修改集合
            list.add("D");
        }
    }
}
```

运行上述代码不会抛出异常，因为 `CopyOnWriteArrayList` 的迭代器是安全失败的。

## ArrayList线程安全的方法？

1. **使用 `Collections.synchronizedList`**

   Java 提供了一个简单的方法来将非线程安全的 `ArrayList` 包装成线程安全的版本。`Collections.synchronizedList` 方法返回一个线程安全的 `List`。

   ```java
   import java.util.ArrayList;
   import java.util.Collections;
   import java.util.List;
   
   public class SynchronizedListExample {
       public static void main(String[] args) {
           List<String> list = new ArrayList<>();
           List<String> synchronizedList = Collections.synchronizedList(list);
   
           synchronizedList.add("A");
           synchronizedList.add("B");
           synchronizedList.add("C");
   
           // 需要在同步块中进行迭代操作
           synchronized (synchronizedList) {
               for (String s : synchronizedList) {
                   System.out.println(s);
               }
           }
       }
   }
   ```

2. **使用CopyOnWriteArrayList**

   `CopyOnWriteArrayList` 是 Java 并发包（`java.util.concurrent`）中的一个线程安全的变体。它适用于读操作远多于写操作的场景，因为每次写操作都会创建一个新的数组副本。

   ```java
   import java.util.List;
   import java.util.concurrent.CopyOnWriteArrayList;
   
   public class CopyOnWriteArrayListExample {
       public static void main(String[] args) {
           List<String> list = new CopyOnWriteArrayList<>();
   
           list.add("A");
           list.add("B");
           list.add("C");
   
           for (String s : list) {
               System.out.println(s);
           }
       }
   }
   ```

## 说一下CopyOnWriteArrayList？

`CopyOnWriteArrayList` 是 Java 并发包（`java.util.concurrent`）中的一个线程安全的 List 实现。它的主要特点是所有的可变操作（如 `add`、`set` 和 `remove` 等）都会在底层数组的一个新副本上执行。这种设计使得 `CopyOnWriteArrayList` 非常适合于读多写少的场景。

**特点和优势**

1. **线程安全**：由于每次修改操作都会创建一个新的数组副本，因此读操作不需要加锁，可以并发进行，而写操作则是互斥的。
2. **无锁读操作**：读操作不需要加锁，因此读操作的性能非常高。
3. **数据一致性**：在迭代过程中，如果有其他线程对列表进行了修改，迭代器仍然可以安全地使用，因为它遍历的是一个稳定的快照。

**使用场景**

`CopyOnWriteArrayList` 适用于以下场景：

- **读多写少**：读操作频繁而写操作较少的场景。
- **迭代稳定性**：需要在迭代过程中避免 `ConcurrentModificationException` 的场景。

**示例代码**

```java
import java.util.List;
import java.util.concurrent.CopyOnWriteArrayList;

public class CopyOnWriteArrayListExample {
    public static void main(String[] args) {
        List<String> list = new CopyOnWriteArrayList<>();

        // 添加元素
        list.add("A");
        list.add("B");
        list.add("C");

        // 迭代元素
        for (String s : list) {
            System.out.println(s);
        }

        // 删除元素
        list.remove("B");

        // 再次迭代元素
        for (String s : list) {
            System.out.println(s);
        }
    }
}
```

**内部实现**

1. **添加操作**：每次添加操作都会创建一个新的数组，并将新元素添加到数组中，然后将新的数组赋值给内部的引用。
2. **删除操作**：删除操作也是创建一个新的数组，并将不需要的元素移除，然后将新的数组赋值给内部的引用。
3. **读操作**：由于读操作不需要加锁，因此非常高效。读操作直接访问内部的数组副本。

**注意事项**

- **内存消耗**：由于每次写操作都会创建一个新的数组副本，因此在写操作频繁的情况下，内存消耗会比较大。
- **性能**：写操作的性能较低，因为每次写操作都需要复制整个数组。

# Map

## 讲一下HashMap？

HashMap提供了基于键值对的存储，并且允许快速的插入、删除和查找操作。底层的数据结构主要是数组+链表的结合，Java8后还引入了红黑树来优化性能。

**数据结构**

<img src="https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/sidebar/sanfene/collection-8.png" alt="三分恶面渣逆袭：JDK 8 HashMap 数据结构示意图" style="zoom:67%;" />

1. **数组：**HashMap的核心是一个数组，称为“桶”（bucket）。每个桶存储一个链表或红黑树的头节点。数组的大小是HashMap的容量，默认初始容量为16
2. **链表：**每个桶存储的元素是一个链表节点。链表节点包含键值对的数据以及指向下一个节点的引用。当发生哈希冲突时（即多个键映射到同一个桶中），新元素会被添加到链表的末尾。
3. **红黑树：**在Java8以后，为了优化性能，当链表中的节点数超过一定阈值（默认是8），链表会转化为红黑树。红黑树是一种自平衡的二叉搜索树，可以显著提高查找和插入的性能。

**工作原理**

1. **哈希函数：**`HashMap` 使用键的哈希码来确定元素存储的位置。具体来说，哈希码经过一系列位运算后，得到一个数组索引。这个过程确保了键值对能够均匀分布在数组中。
2. **哈希冲突：**当两个不同的键映射到同一个桶时，就会发生哈希冲突。`HashMap` 通过链表或红黑树来解决冲突。新元素会被添加到链表的末尾或红黑树中。
3. **扩容：**当 `HashMap` 中的元素数量超过一定比例（称为负载因子，默认值为 0.75）时，`HashMap` 会进行扩容操作。扩容时，数组的容量会翻倍，并且所有元素会被重新哈希到新的数组位置。这是一个比较耗时的操作，但它确保了 `HashMap` 的性能。

**小结**

`HashMap` 通过数组、链表和红黑树的结合，实现了高效的键值对存储和查找。其哈希函数和扩容机制确保了元素的均匀分布和性能的稳定性。尽管 `HashMap` 不是线程安全的，但它在单线程环境中表现出色，是 Java 集合框架中非常重要的一部分。

## 为什么选则红黑树优化性能，而不是二叉树/平衡树？

选择红黑树而不是普通的二叉树或其他平衡树（如 AVL 树）来优化 `HashMap` 的性能，主要是基于以下几个方面的考虑：

**红黑树的特性**

1. **自平衡：**红黑树是一种自平衡的二叉搜索树，它通过严格的颜色规则（红色和黑色节点）来保证树的高度近似平衡。这样可以确保插入、删除和查找操作的时间复杂度为 O(log n)。
2. **插入和删除操作效率**：相比于其他平衡树（如 AVL 树），红黑树在插入和删除操作时，旋转和调整的次数较少。虽然 AVL 树在查找操作上可能更快一些（因为它更加严格地平衡），但是在插入和删除操作上，红黑树的效率更高。
3. **适度的平衡**：红黑树的平衡性相对较弱一些，这意味着它允许一定程度的不平衡，这使得插入和删除操作的开销相对较低。对于 `HashMap` 来说，这种适度的平衡性在实际应用中表现出更好的综合性能。

**为什么不选二叉树？**

普通的二叉树（即非平衡二叉搜索树）在最坏情况下会退化成链表，导致查找、插入和删除操作的时间复杂度变为 O(n)。这与 `HashMap` 设计的高效性目标相悖。因此，普通的二叉树并不适合用于 `HashMap` 的底层数据结构。

**为什么不选AVL树？**

1. **插入和删除效率**：AVL 树是一种严格平衡的二叉搜索树，它在每次插入和删除操作后都会进行严格的平衡调整。这意味着 AVL 树可能需要更多的旋转操作来保持平衡，从而增加了插入和删除操作的开销。
2. **综合性能**：虽然 AVL 树在查找操作上可能稍微快一些，但 `HashMap` 更加关注插入和删除操作的性能。红黑树在这些操作上表现出更好的综合性能，因此更适合用于 `HashMap`。

**小结**

选择红黑树作为 `HashMap` 的底层数据结构之一，是因为红黑树在保证树高度近似平衡的同时，能够在插入和删除操作上提供更高的效率。相比于普通的二叉树和其他平衡树（如 AVL 树），红黑树在综合性能上更符合 `HashMap` 的需求，能够在各种操作中提供更稳定和高效的性能表现。

## 红黑树原理？

红黑树是一种自平衡二叉搜索树，通过在插入和删除节点时进行适当的旋转和重新着色操作来保持树的平衡。它的定义和操作规则使得树的高度保持在 O(log n) 的范围内，从而确保插入、删除和查找操作的高效性。

**红黑树的性质**

红黑树通过以下性质来保证相对平衡：

1. **每个节点要么是红色，要么是黑色。**
2. **根节点是黑色的。**
3. **每个叶子节点（NIL 节点，表示空节点）是黑色的。**
4. **如果一个节点是红色的，则它的两个子节点都是黑色的。**
5. **从任一节点到其每个叶子节点的所有路径都包含相同数目的黑色节点。**

**插入操作**

插入节点时，红黑树首先按照二叉搜索树的插入方法插入新节点，然后通过重新着色和旋转操作来修复违反红黑树性质的情况。具体步骤如下：

1. **插入新节点**：将新节点插入到树中，初始颜色为红色。
2. **修复树**：通过重新着色和旋转操作来修复红黑树性质的破坏。
   - 如果新节点的父节点是黑色的，不需要任何操作。
   - 如果新节点的父节点是红色的，可能会违反红黑树的性质，需要进行修复。修复的具体操作取决于新节点的叔叔节点的颜色和位置，分为以下几种情况：
     - **叔叔节点是红色**：重新着色。
     - **叔叔节点是黑色**：根据新节点的位置进行旋转（左旋或右旋），然后重新着色。

**删除操作**

删除节点时，红黑树首先按照二叉搜索树的删除方法删除节点，然后通过重新着色和旋转操作来修复违反红黑树性质的情况。具体步骤如下：

1. **删除节点**：找到并删除节点。
2. **修复树**：通过重新着色和旋转操作来修复红黑树性质的破坏。
   - 如果删除的是红色节点，不需要任何操作。
   - 如果删除的是黑色节点，可能会违反红黑树的性质，需要进行修复。修复的具体操作取决于删除节点的兄弟节点的颜色和位置，分为以下几种情况：
     - **兄弟节点是红色**：重新着色并旋转。
     - **兄弟节点是黑色**：根据兄弟节点的子节点颜色进行不同的旋转和重新着色操作。

**旋转操作**

旋转操作是红黑树修复过程中最重要的操作之一，分为左旋和右旋：

- **左旋**：以某个节点为支点，将其右子节点提升为新的根节点，原根节点成为新根节点的左子节点。
- **右旋**：以某个节点为支点，将其左子节点提升为新的根节点，原根节点成为新根节点的右子节点。

通过这些旋转操作，红黑树能够有效地修复结构失衡的情况，从而保持树的高度在 O(log n) 的范围内。

**总结**

红黑树通过严格的定义和插入、删除操作中的重新着色和旋转操作，保证了树的相对平衡。这样，红黑树能够在最坏情况下仍然保持 O(log n) 的时间复杂度，从而确保插入、删除和查找操作的高效性。

## HashMap的put流程？

1. **计算哈希值：**

   使用键的 `hashCode` 方法计算键的哈希值，并通过一些位运算将哈希值进一步混淆，以减少哈希冲突。

   ```java
   int hash = hash(key);
   ```

2. **确认桶的位置：**

   使用哈希值和桶数组的长度确定键应该放入的桶的位置。

   ```java
   int bucketIndex = (n - 1) & hash;
   ```

3. **检查桶内的链表或红黑树**：

   - 如果桶为空，则创建一个新的节点并放入桶中。
   - 如果桶不为空，则遍历桶内的链表或红黑树，检查是否有相同的键：
     - 如果找到相同的键，则更新该键对应的值。
     - 如果没有找到相同的键，则将新节点插入到链表或红黑树中。

4. **处理哈希冲突**

   如果链表长度超过阈值（默认是8），则将链表转换为红黑树，以提高性能。

5. **调整容量**

   如果插入新节点后，`HashMap` 的实际大小超过了阈值（`load factor * capacity`），则需要进行扩容（rehash），即创建一个更大的桶数组，并将所有节点重新分配到新的桶中。

## HashMap查找元素？

1. **计算哈希值**
   - 使用键的 `hashCode` 方法计算键的哈希值，并通过一些位运算将哈希值进一步混淆，以减少哈希冲突。
2. **确认桶的位置**
   - 使用哈希值和桶数组的长度确定键应该查找的桶的位置。
3. **遍历桶内的链表或红黑树**
   - 获取桶中的第一个节点。
   - 遍历桶内的链表或红黑树，查找与键匹配的节点：
     - 如果找到相同的键，则返回该键对应的值。
     - 如果没有找到相同的键，则返回 `null`。

## HashMap的哈希函数？

`HashMap` 的哈希函数设计是为了尽可能均匀地分布键值对，以减少哈希冲突。Java 中 `HashMap` 的哈希函数设计非常精巧，通过混淆和位运算来优化哈希值的分布。以下是 `HashMap` 中哈希函数的设计原理和实现方法。

**哈希函数设计原理**

1. **减少哈希冲突**：通过混淆原始哈希值，尽量使得不同的键分布在不同的桶中。
2. **提高访问效率**：通过位运算来快速计算桶的位置。

**哈希函数实现**

Java 8 中 `HashMap` 的哈希函数实现如下：

```java
static final int hash(Object key) {
    int h;
    // 计算 key 的 hashCode，并进行高低位混淆
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

**解释**

1. **计算原始哈希值：**

   - `key.hashCode()` 计算键的原始哈希值 `h`。

   - ```java
     h = key.hashCode();
     ```

2. **高低位混淆：**

   - 使用 XOR 操作将哈希值的高 16 位和低 16 位混淆，以增加哈希值的随机性，减少哈希冲突。

   - ```java
     h ^ (h >>> 16);
     ```

   - 具体来说，`h >>> 16` 是将 `h` 无符号右移 16 位，即取高 16 位的值，然后与 `h` 进行异或操作。��种混淆方法可以使得哈希值的高位信息也参与到桶位置的计算中，避免低位信息不足导致的哈希冲突。

**桶位置计算**

哈希值计算完成后，通过以下方式计算桶的位置：

```java
int bucketIndex = (n - 1) & hash;
```

其中 `n` 是桶数组的长度（通常是 2 的幂），`hash` 是混淆后的哈希值。`(n - 1) & hash` 通过位运算快速计算出桶的位置。

## 为何HashMap 的容量是 2 的倍数？

`HashMap` 的容量（即桶数组的长度）通常是 2 的幂，这是为了优化哈希值到桶索引的映射，使得计算更加高效。具体原因如下：

**高效的哈希值映射**

`HashMap` 使用位运算来计算哈希值对应的桶索引：

```java
int bucketIndex = (n - 1) & hash;
```

其中 `n` 是桶数组的长度，`hash` 是混淆后的哈希值。

1. **位运算的高效性**：
   - `&` 操作符比取模运算 `%` 更高效。位运算直接操作二进制位，速度非常快。
   - 当 `n` 是 2 的幂时，`n - 1` 的二进制表示全是 1。例如，`n = 16` (2 的 4 次方)，`n - 1 = 15`，二进制表示为 `00001111`。
2. **均匀分布**：
   - 当 `n` 是 2 的幂时，`(n - 1) & hash` 能够均匀地分布哈希值，减少哈希冲突。
   - 例如，如果 `n = 16`，则 `hash & 15` 取哈希值的低 4 位，这样可以确保哈希值均匀分布在 0 到 15 之间。

## 为什么 HashMap 链表转红黑树的阈值为 8 呢？

8是权衡的结果。

- 既能在链表长度较小时保持较低的空间开销，又能在链表长度较大时提升查找性能。
- 实际上哈希冲突的可能性较小
- 避免链表和红黑树的频繁转换

## 为什么HashMap扩容的负载因子是0.75？

1. **性能与空间的平衡：**

   - **性能**：较低的负载因子意味着 `HashMap` 中有更多的空桶，这减少了哈希冲突的概率，从而提高了查找、插入和删除操作的性能。
   - **空间**：较高的负载因子意味着 `HashMap` 中的桶利用率更高，减少了空间浪费。

   0.75 是一个折中的选择，它在性能和空间利用率之间找到了一个平衡点。较高的负载因子（如 1.0）会导致更多的哈希冲突，从而降低性能；较低的负载因子（如 0.5）会导致更多的空间浪费。

2. **经验和统计数据**：

   - 经过大量的实际应用和测试，0.75 被证明是一个能够在大多数情况下提供良好性能和空间利用率的负载因子。
   - 这个值已经在许多实际应用中得到了验证，能够在大多数情况下提供较好的性能。

3. **避免频繁扩容**：

   - 如果负载因子设定得太低，`HashMap` 会频繁地进行扩容操作，这会带来额外的系统开销。
   - 0.75 作为一个较高的负载因子，减少了扩容的频率，从而降低了扩容带来的性能开销。

**总结**

选择 0.75 作为 `HashMap` 的默认负载因子是基于性能和空间利用率的综合考虑。这个值在大多数情况下能够提供较好的查找、插入和删除性能，同时又能有效利用空间，避免频繁扩容带来的额外开销。经过大量的实际应用和测试，0.75 被证明是一个合理且高效的选择。

## HashMap的扩容机制？

`HashMap` 的扩容机制是为了在元素数量增加到一定程度时，确保其性能不会显著下降。扩容机制涉及到重新分配存储桶（buckets），并将现有元素重新分布到新的存储桶中。以下是 `HashMap` 扩容机制的详细步骤和原理：

1. **触发条件**

   `HashMap` 的扩容是由负载因子（load factor）和当前容量决定的。当 `HashMap` 中的元素数量超过 `capacity * loadFactor` 时，就会触发扩容。

   - **默认负载因子**：0.75
   - **默认初始容量**：16

2. **扩容过程**

   当触发扩容时，`HashMap` 会进行以下步骤：

   1. **计算新容量**：
      - 新的容量通常是当前容量的两倍。
      - 如果当前容量已经达到了最大值（`Integer.MAX_VALUE`），则不会再扩容。
   2. **分配新的存储桶数组**：
      - 创建一个新的数组，大小为新的容量。
   3. **重新哈希**：
      - 将旧存储桶中的每个元素重新计算哈希值，并放入新的存储桶中。
      - 重新哈希的目的是为了将元素更均匀地分布在新的存储桶中，减少哈希冲突。

3. **重新哈希的原因**

   重新哈希的主要原因是因为哈希值的计算与存储桶的数量相关。当存储桶数量改变时，原有的哈希值可能不再适合新的存储桶分布，需要重新计算以确保哈希分布的均匀性。

4. **扩容的影响**

   - **性能**：扩容是一个相对耗时的操作，因为需要重新计算所有元素的哈希值并重新分配位置。然而，这种操作并不频繁发生（通常是指数级增长），因此对总体性能的影响是可控的。
   - **空间**：扩容增加 `HashMap` 的存储空间，使其能够容纳更多的元素。

**总结**

`HashMap` 的扩容机制是通过重新分配存储桶数组和重新计算哈希值来实现的。扩容的触发条件是元素数量超过 `capacity * loadFactor`。尽管扩容操作比较耗时，但它在 `HashMap` 中并不频繁发生，而且通过这种机制可以确保 `HashMap` 在高负载情况下仍然具有良好的性能。

## HashMap线程安全吗？

`HashMap` 在 Java 中并不是线程安全的。它设计的初衷是为了在单线程环境中使用。如果在多线程环境中使用 `HashMap`，而没有进行适当的同步措施，可能会导致数据不一致、死循环等问题。下面是一些具体的原因和替代方案。

**为什么HashMap不是线程安全的**

1. **结构修改**：在多线程环境中，如果多个线程同时对 `HashMap` 进行修改（例如插入或删除元素），可能会导致内部数据结构（如链表或树）的不一致。
2. **扩容**：`HashMap` 在扩容时，会重新分配存储桶并重新哈希现有元素，如果在扩容过程中有其他线程进行插入操作，可能会导致死循环或数据丢失。

**线程安全的替代方案**

1. `Collections.synchronizedMap`

   Java 提供了一个简单的方法来使 `HashMap` 线程安全，即通过 `Collections.synchronizedMap` 方法：

   ```java
   Map<K, V> synchronizedMap = Collections.synchronizedMap(new HashMap<K, V>());
   ```

   使用 `synchronizedMap` 包装后的 `Map` 会在每个操作上添加同步锁，从而保证线程安全。但这种方式的性能可能较差，因为所有操作都需要获取同一个锁。

2. `ConcurrentHashMap`

   `ConcurrentHashMap` 是 Java 提供的一个线程安全的哈希表实现，专为高并发环境设计。它通过分段锁（Segmented Locking）来提高并发性能：

   ```java
   ConcurrentHashMap<K, V> concurrentMap = new ConcurrentHashMap<>();
   ```

   `ConcurrentHashMap` 的一些特点：

   - **分段锁**：内部将数据分成多个段，每个段有自己的锁，从而允许多个线程并发访问不同段的数据。
   - **无锁读取**：大多数读取操作是无锁的，性能更高。
   - **更好的并发性**：相比于 `Collections.synchronizedMap`，`ConcurrentHashMap` 在高并发环境下表现更好。

## LinkedHashMap怎么实现有序？

`LinkedHashMap` 是 `HashMap` 的一个子类，它在保持哈希表特性的同时，还维护了一个双向链表，以保证元素的插入顺序（或访问顺序）。`LinkedHashMap` 的有序性就是通过这个双向链表来实现的。

**主要实现细节**

1. **双向链表**：
   `LinkedHashMap` 通过在每个节点中增加两个指针（`before` 和 `after`）来维护一个双向链表。这两个指针分别指向前一个节点和后一个节点。

2. **节点类**：
   `LinkedHashMap` 使用一个内部类 `Entry` 来表示节点，这个类继承自 `HashMap` 的 `Node` 类，并增加了 `before` 和 `after` 指针。

   java 复制代码

   ```java
   static class Entry<K,V> extends HashMap.Node<K,V> {
       Entry<K,V> before, after;
       Entry(int hash, K key, V value, Node<K,V> next) {
           super(hash, key, value, next);
       }
   }
   ```

3. **头尾指针**：
   `LinkedHashMap` 维护两个指针 `head` 和 `tail`，分别指向双向链表的头部和尾部。这两个指针用于快速访问链表的两端。

4. **插入顺序**：
   当新元素插入到 `LinkedHashMap` 中时，新节点会被添加到双向链表的尾部。这样保证了元素的插入顺序。

5. **访问顺序（可选）**：
   `LinkedHashMap` 还可以通过构造函数参数 `accessOrder` 来决定是否按访问顺序排序。如果 `accessOrder` 为 `true`，则每次访问一个元素（通过 `get` 方法）时，该元素会被移动到链表的尾部。

## TreeMap怎么实现有序？

`TreeMap` 是 Java 中实现 `NavigableMap` 接口的一个类，它基于红黑树（Red-Black Tree）实现。红黑树是一种自平衡的二叉搜索树，通过在插入和删除操作后进行必要的旋转和重新着色，保证树的高度始终保持在 O(log n) 的范围内，从而使得查找、插入和删除操作的时间复杂度均为 O(log n)。

**实现有序的机制**

1. **红黑树结构**：
   - `TreeMap` 内部使用红黑树来存储键值对。
   - 红黑树是一种特殊的二叉搜索树，它通过在插入和删除操作后进行必要的旋转和重新着色，保持树的平衡。
2. **键的自然顺序或比较器**：
   - `TreeMap` 通过键的自然顺序（即键实现了 `Comparable` 接口并定义了 `compareTo` 方法）或通过提供的比较器（`Comparator`）来确定键的顺序。
   - 在插入元素时，`TreeMap` 使用键的比较结果来决定元素在树中的位置，从而保证树的有序性。

# Set

## HashSet的底层实现？

`HashSet` 是 Java 集合框架中的一个类，用于存储不重复的元素。它的底层实现依赖于 `HashMap`。下面是对 `HashSet` 底层实现的详细解释。

**`HashSet`的底层实现**

1. **基本结构**

`HashSet` 内部使用一个 `HashMap` 来存储所有的元素。具体来说，每当你向 `HashSet` 添加一个元素时，这个元素会作为 `HashMap` 的键插入，而值则是一个固定的对象（通常是 `PRESENT`，一个静态的 `Object` 实例）。

**2. 主要字段**

`HashSet` 的主要字段如下：

```java
private transient HashMap<E, Object> map;

// Dummy value to associate with an Object in the backing Map
private static final Object PRESENT = new Object();
```

3. **构造方法**

`HashSet` 提供了多个构造方法，其中一个最常用的构造方法如下：

```java
public HashSet() {
    map = new HashMap<>();
}
```

这个构造方法初始化了一个空的 `HashMap`。

4. **添加元素**

当你调用 `HashSet` 的 `add` 方法时，实际上是将元素作为键插入到 `HashMap` 中，值为 `PRESENT`：

```java
public boolean add(E e) {
    return map.put(e, PRESENT) == null;
}
```

5. **其他方法**

其他方法如 `remove`、`contains` 等也都是通过操作内部的 `HashMap` 来实现的。例如：

- `remove` 方法：

```java
public boolean remove(Object o) {
    return map.remove(o) == PRESENT;
}
```

- `contains` 方法：

```java
public boolean contains(Object o) {
    return map.containsKey(o);
}
```

**工作原理**

1. **哈希计算**：当你向 `HashSet` 添加一个元素时，首先会计算这个元素的哈希值。
2. **哈希冲突处理**：如果两个元素的哈希值相同，`HashMap` 会使用链表或红黑树来处理哈希冲突。
3. **不重复性**：由于 `HashMap` 的键是唯一的，所以 `HashSet` 中的元素也是唯一的。

**总结**

- `HashSet` 使用 `HashMap` 作为底层数据结构。
- 元素作为 `HashMap` 的键存储，值为一个固定的对象 `PRESENT`。
- 通过 `HashMap` 的特性，`HashSet` 实现了元素的唯一性和高效的查找、插入和删除操作。

这种设计使得 `HashSet` 具有较高的性能，尤其是在需要快速查找和去重的场景下。